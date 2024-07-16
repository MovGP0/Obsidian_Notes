## Preparation

Add NuGet packages
```xml
<ItemGroup>
    <PackageReference Include="Microsoft.CSharp" Version="4.7.0" />
</ItemGroup>
```

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
    .AddSyntaxTrees(syntaxTree)
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

## Syntax Rewriting

Implement Syntax Rewriter
```csharp
public sealed class MyRewriter : CSharpSyntaxRewriter
{
    private readonly SemanticModel SemanticModel;

    public MyRewriter(SemanticModel semanticModel)
	    => SemanticModel = semanticModel;

	public override SyntaxNode VisitLocalDeclarationStatement(LocalDeclarationStatementSyntax node)
	{
	    // ...
	}
}
```

Visit and rewrite syntax trees
```csharp
Compilation compilation = /* ... */;
foreach (var syntaxTree in compilation.SyntaxTrees)
{
    SemanticModel model = compilation.GetSemanticModel(sourceTree);
    CSharpSyntaxRewriter rewriter = new MyRewriter(model);
    SyntaxNode newSource = rewriter.Visit(sourceTree.GetRoot());
	// TODO: update code
}
```

Replace source code file with rewritten code
```csharp
if (newSource != sourceTree.GetRoot())
{
	File.WriteAllText(sourceTree.FilePath, newSource.ToFullString());
}
```

## References

- [Get started with syntax transformation](https://learn.microsoft.com/en-us/dotnet/csharp/roslyn-sdk/get-started/syntax-transformation)

## See also

* [[Load and Execute Code Analyzers]]
* [[Implement Roslyn Analyzer]]