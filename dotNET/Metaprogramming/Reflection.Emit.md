`Reflection.Emit` is an API to dynamically create IL code. Typical use cases include

- Domain Specific Languages (ie. Regex)
- Custom Compilers
- Mocking Frameworks for unit testing
- Generate Serialization Code from Reflection data

## Create dynamic assembly

```csharp
AssemblyName assemblyName = new AssemblyName
{
    Name = "MyAssembly"
};

AssemblyBuilder assemblyBuilder = AppDomain.CurrentDomain.DefineDynamicAssembly(assemblyName);

ModuleBuilder moduleBuilder = assemblyBuilder.DefineDanymicModule("MyModule");
```

### Create methods

Get type information for existing types we want to use:
```csharp
// StringBuilder()
var stringBuilderConstructor = typeof(StringBuilder).GetConstructor(Type.EmptyTypes);

// StringBuilder.Append(string)
var appendMethod = typeof(StringBuilder).GetMethod("Append", new Type[] { typeof(string) });

// StringBuilder.ToString()
var toStringMethod = typeof(StringBuilder).GetMethod("ToString", Type.EmptyTypes);
```

Implement custom method
```csharp
var type = moduleBuilder.DefineType("MyNamespace.MyType");

var myMethod = type.DefineMethod("MyMethod", MethodAttributes.Static | MethodAttributes.Public, typeof(string), new Type[] { paramType });

var generator = myMethod.GetILGenerator;

// Example: var sb = new StringBuilder();
generator.Emit(OpCodes.Newobj, stringBuilderConstructor);
generator.Emit(OpCodes.Stlloc_0);
generator.Emit(OpCodes.Ldloc_0);

// Example: sb.Append("foo");
generator.Emit(OpCodes.Ldstr, "foo");
generator.Emit(OpCodes.Callvirt, appendMethod);
generator.Emit(OpCodes.Ldarg_0);

// Example: return sb.ToString();
generator.Emit(OpCodes.Pop);
generator.Emit(OpCodes.Ldloc_0);
generator.Emit(OpCodes.Callvirt, toStringMethod);
generator.Emit(OpCodes.Ret);

// Example: return string.Empty;
generator.Emit(OpCodes.Ldstr, "");
generator.Emit(OpCodes.Ret);

generator.Generate(target);
```

## Add debugging support

The [DebuggableAttribute](https://learn.microsoft.com/en-us/dotnet/api/system.diagnostics.debuggableattribute) needs to be added for the .NET Runtime to track debugging information.

```csharp
using System.Diagnostics;

var debugAttribute = typeof(DebuggableAttribute);
var debugCtor = debugAttribute.GetConstructor(new Type[] { typeof(DebuggableAttribute.DebuggingModes) });
var debugBuilder = new CustomAttributeBuilder(debugCtor, new object[] { DebuggableAttribute.DebuggingModes.DisableOptimizations | DebuggableAttribute.DebuggingModes.Default });
assembly.SetCustomAttribute(debugBuilder);
```

Name method parameters
```csharp
method.DefineParameter(1, ParameterAttributes.In, "name"); // first parameter is now named "name"
```

Name variables
```csharp
toStringLocal.SetLocalSymInfo("builder");
```

Link OpCodes with source code
```csharp
generator.MarkSequencePoint(document, lineNumberStart, startIndex, lineNumberEnd, endIndex);
generator.Emit(opcode, @value); // this opcode is now linked with the source location
```

For interception of method calls for debugging, consider using [Castle DynamicProxy](https://github.com/castleproject/Home).

## Verify MSIL code

The IL Generator can emit invalid code. When calling invalid code an `InvalidProgramException` is raised.

To check the code the [PE Verify Tool](https://learn.microsoft.com/en/dotnet/framework/tools/peverify-exe-peverify-tool) (via the [AssemblyVerifier NuGet](https://www.nuget.org/packages/AssemblyVerifier)) can be used to check the types in IL code before executing it.

Usage
```bash
dotnet add AssemblyVerifier
```

```csharp
assembly.Save(@"C:\foobar\MyAssembly.dll");
AssemblyVerification.Verify(assembly);
```

## See also

- [[Intermediate Language]]
- [Mono.Cecil](https://github.com/jbevain/cecil)
