Namespace imports
```csharp
using Microsoft.CodeAnalysis;  
using Microsoft.CodeAnalysis.CSharp;
```

## Parsing

Parse Syntax Tree
```csharp
SyntaxTree? syntaxTree = CSharpSyntaxTree.ParseText(code);
```

**Note** The `SyntaxTree` is a .NET syntax tree and language independent. Compilers may not support all the node types defined in the syntax tree.

## Compilation

References to other assemblies
```csharp
var currentAssemblyPath = Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location)!;

var references = new List<MetadataReference>  
{  
    MetadataReference.CreateFromFile(typeof(object).Assembly.Location),  // mscorlib.dll
    MetadataReference.CreateFromFile(typeof(DynamicAttribute).Assembly.Location),  // System.Core.dll
    MetadataReference.CreateFromFile(typeof(Microsoft.CSharp.RuntimeBinder.CSharpArgumentInfo).Assembly.Location), // dynamic parameters/values at runtime
    MetadataReference.CreateFromFile(Path.Combine(currentAssemblyPath, "Custom.dll")) // other DLL that needs to be references
};
```

C# Compiler options
```csharp
var compileOptions = new CSharpCompilationOptions(  
    OutputKind.DynamicallyLinkedLibrary,  
    allowUnsafe: false,  
    checkOverflow: true,  
    optimizationLevel: OptimizationLevel.Debug);
```

Create compilation
```csharp
var compilation = CSharpCompilation  
    .Create("DynamicCompilation")  
    .WithOptions(compileOptions)  
    .AddReferences(references)  
    .AddSyntaxTrees(syntaxTree);
```

**Note:** consider `CSharpCompilation.CreateScriptCompilation()` for scripting.

Emit IL code to In-Memory stream
```csharp
using var ms = new MemoryStream();  
var result = compilation.Emit(ms);  
ms.Seek(0, SeekOrigin.Begin);
```

Load the In-Memory assembly
```csharp
var assembly = Assembly.Load(ms.ToArray());
```
