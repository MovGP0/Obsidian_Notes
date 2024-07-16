Add NuGet [Microsoft.CodeAnalysis.CSharp.Scripting](https://www.nuget.org/packages/Microsoft.CodeAnalysis.Scripting/)

Namespaces
```csharp
using Microsoft.CodeAnalysis.Analyzers;
using Microsoft.CodeAnalysis.Common;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.Scripting.Common;
```

```csharp
string source = "...";
var options = ScriptOptions.Default;

// add references
options.WithReferences(Assemby.From(new AssemblyName("CustomAssembly"))));
options.WithImports("System", "System.Text");

// custom assembly loader
using var loader = new InteractiveAssemblyLoader();

var script = CSharpScript.Create(source, options, assemblyLoader: loader);
var result = await script.RunAsync();
```

Precompilation
```csharp
script.Compile();
```

Access Syntax Tree
```csharp
Compilation compilation = script.GetCompilation();
```

Global variables
```csharp
// Note: the type must be located in a referenced assembly
var globals = new Person(firstName: "Max", lastName: "Mustermann");
```

Evaluate script
```csharp
object result = await CSharpScript.EvaluateAsync("1 + 2", globals: globals);
int result = await CSharpScript.EvaluateAsync<int>("1 + 2", globals: globals);
```

## See also

- [Scripting API Samples](https://github.com/dotnet/roslyn/blob/main/docs/wiki/Scripting-API-Samples.md)
