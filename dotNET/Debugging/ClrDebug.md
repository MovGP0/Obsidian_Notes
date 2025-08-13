## Setup

```sh
dotnet add package ClrDebug
```

```csharp
using ClrDebug;
using static ClrDebug.Extensions;
```

## Launch or attach to process

Setup
```csharp
var corDebug = new CorDebug();
var callbacks = new CorDebugManagedCallback();
corDebug.SetManagedHandler(callbacks);
```

Launch process with the debugger attached
```csharp
var process = corDebug.CreateProcess(
    applicationName: "C:\\Path\\To\\YourApp.exe",
    commandLine: null,
    dwCreationFlags: CreateProcessFlags.CREATE_NEW_CONSOLE);
```

Attach to an existing process:
```csharp
var process = corDebug.DebugActiveProcess(processId: 12345, fWin32Attach: false);
```

Register Callbacks
```csharp
callbacks.OnAnyEvent += (sender, e) =>
{
    Console.WriteLine($"Event: {e.Kind}");

    if (e.Kind != CorDebugManagedCallbackKind.Breakpoint && e.Kind != CorDebugManagedCallbackKind.StepComplete)
    {
        e.Controller.Continue(fIsOutOfBand: false);
    }
};
```

### .NET Core/.NET 5+

When attaching to an running .NET Core process, we need to use the `DbgShim`:

```csharp
// process ID of the target process
var processId = 12345;

var startupEvent = DbgShim.GetStartupNotificationEvent();
var clrs = DbgShim.EnumerateCLRs(processId);

// get the .NET version string
var version = DbgShim.CreateVersionStringFromModule(processId, clrs[0].ModuleBaseAddress);

// create the debugger for the required .NET version
ICorDebug icd = DbgShim.CreateDebuggingInterfaceFromVersionEx(version);

var corDebug = new CorDebug(icd);
corDebug.Initialize();
corDebug.SetManagedHandler(callbacks);
corDebug.DebugActiveProcess(debuggeeProcessId, fWin32Attach: false);
```

## Set a breakpoint

> [!note]
> For the mapping we need a "Program Database" file (`*.pdb`).

### Set breakpoint at method entry point

Set a breakpoint at the entry-point of a given method:
```csharp
callbacks.OnLoadModule += (sender, args) =>
{
    var module = args.Module; // CorDebugModule
    var meta = module.GetMetaDataInterface<MetaDataImport>(); // IMetaDataImport

    // Resolve type and method tokens
    var typeToken = meta.FindTypeDefByName("TargetNamespace.TargetType", mdToken NilToken);

    // Find method by name (simple filter; for overloads, check sig)
    var methodToken = meta.EnumMethods(typeToken).First(t =>
    {
        var props = meta.GetMethodProps(t);
        return props.name == "TargetMethod";
    });

    // Get ICorDebugFunction and create a function-entry breakpoint
    var function = module.GetFunctionFromToken(methodToken);
    var breakPoint = function.CreateBreakpoint(); // entry-point breakpoint
    breakPoint.Activate(true); // enable it

    Console.WriteLine("Function breakpoint set on TargetNamespace.TargetType.TargetMethod");
};
```

### Set a breakpoint at a given line number

Set a breakpoint at a given line. We need to map the file name and line number to the IL offset.
```csharp
void SetSourceLineBreakpoint(CorDebugModule module, string filePath, int lineNumber1Based)
{
    if (lineNumber1Based < 1) throw new ArgumentOutOfRangeException("Line numbers start at 1 for counting");

    // Load the symbol reader for the module (PDB)
    var binder = new SymBinder(); // ISymUnmanagedBinder
    var mdi = module.GetMetaDataInterface<MetaDataImport>();
    var modulePath = module.GetName();

    // Open reader from file; alternatively use OpenSymReaderFromAddress etc.
    var reader = binder.GetReaderForFile(modulePath, symbolSearchPath: null);

    // Iterate all methods; find the first sequence point matching file+line
    foreach (var type in mdi.EnumTypeDefs())
    {
        foreach (var methodToken in mdi.EnumMethods(type))
        {
            var symMethod = reader.GetMethod(methodToken, throwIfNotFound: false);
            if (symMethod == null) continue;

            var seqPoints = symMethod.GetSequencePoints(); // IL offsets ↔ source positions
            var match = seqPoints.FirstOrDefault(sp =>
                string.Equals(sp.Document.URL, filePath, StringComparison.OrdinalIgnoreCase) &&
                sp.StartLine == lineNumber1Based);

            if (match == null) continue;

            // Map to ICorDebugCode and set IL breakpoint at offset
            var function = module.GetFunctionFromToken(methodToken);
            var code = function.GetILCode();               // ICorDebugCode (IL)
            var ilOffset = (uint)match.Offset;
            var bp = code.CreateBreakpoint(ilOffset);      // offset breakpoint
            bp.Activate(true);

            Console.WriteLine($"Source breakpoint set: {filePath}:{lineNumber1Based} (IL {ilOffset})");
            return;
        }
    }

    throw new InvalidOperationException("No sequence point found for requested file:line.");
}
```

Now apply the breakpoint:
```csharp
callbacks.OnLoadModule += (s, e) =>
{
    if (e.Module.IsInMemory) return; // needs a file path for this simple reader
    SetSourceLineBreakpoint(e.Module, @"C:\src\MyApp\Program.cs", 42);
};
```

## Handle breakpoint hit

```csharp
callbacks.OnBreakpoint += (s, e) =>
{
    var process = e.Controller as CorDebugProcess;
    var thread = e.Thread; // CorDebugThread

    Console.WriteLine("=== Breakpoint hit ===");

    // Active frame + full managed call stack
    var frame = thread.GetActiveFrame(); // top frame
    foreach (var f in thread.EnumerateFrames()) // includes IL/native/internal
    {
        if (f.IsILFrame)
        {
            var il = f.As<CorDebugILFrame>();
            var function = il.Function;
            var className = function.Class.GetTokenAndScope().Item2.GetTypeDefProps(function.Class.Token).name;
            var methodName = function.GetTokenAndScope().Item2.GetMethodProps(function.Token).name;
            Console.WriteLine($"  at {className}.{methodName} IL={il.GetIP().ilOffset}");
        }
        else
        {
            Console.WriteLine($"  [non-IL frame: {f.GetType().Name}]");
        }
    }

    // Read arguments and locals (IL frame)
    if (frame.IsILFrame)
    {
        var il = frame.As<CorDebugILFrame>();

        // Dump Arguments
        var il4 = il.QueryInterfaceOrNull<CorDebugILFrame4>();
        if (il4 != null)
        {
            foreach (var arg in il4.EnumerateArguments())
            {
                DumpValue("arg", arg);
            }
        }

        // Dump local variables
        uint slot = 0;
        while (true)
        {
            try
            {
                var local = il.GetLocalVariable(slot); // ICorDebugValue
                DumpValue($"local[{slot}]", local);
                slot++;
            }
            catch (COMException)
            {
                break; // no more locals
            }
        }
    }

    // TODO: Stay paused so you can inspect more

	// Resume using:
    process.Continue(false);
};
```

```csharp
static void DumpValue(string label, CorDebugValue value)
{
    if (value is CorDebugGenericValue gen)            // primitives & enums
    {
        object? boxed = gen.Dereference()?.GetBoxedValue(); // convenience
        Console.WriteLine($"    {label} = {boxed ?? "<null>"}");
    }
    else if (value is CorDebugStringValue str)
    {
        Console.WriteLine($"    {label} = \"{str.String}\"");
    }
    else if (value is CorDebugObjectValue obj)
    {
        var cls = obj.Class;
        Console.WriteLine($"    {label} = object of {cls?.Token:X8}");
        // You can read fields via obj.GetFieldValue(class/fieldToken) etc.
    }
    else
    {
        Console.WriteLine($"    {label} = ({value.GetType().Name})");
    }
}
```

## Continue/step debugging

```csharp
callbacks.OnBreakpoint += (s, e) =>
{
    var thread = e.Thread;

    // Single-step over the next IL instruction:
    var il = thread.GetActiveFrame().As<CorDebugILFrame>();
    var steppable = il.CreateStepper(); // ICorDebugStepper
    steppable.SetUnmappedStopMask(CorDebugUnmappedStop.STOP_NONE);
    steppable.Step(fStepIn: false); // false = Step Over, true = Step Into

    // The runtime will deliver OnStepComplete next
    e.Controller.Continue(false);
};

callbacks.OnStepComplete += (s, e) =>
{
    Console.WriteLine("Step complete at new IP.");
    // Inspect state if desired, then continue or wait for UI input
    // e.Controller.Continue(false);
};
```
