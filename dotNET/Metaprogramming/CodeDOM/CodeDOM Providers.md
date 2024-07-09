.NET `CodeDomProvider`s are typically located in the global assembly cache.

List the available code providers:

```csharp
using System.CodeDom.Compiler;

foreach (CompilerInfo ci in CodeDomProvider.GetAllCompilerInfo())
{
    Console.WriteLine(string.Join("; ", ci.GetLanguages()));
}
```

Load a CodeDOM Provider:

```csharp
CodeDomProvider cSharpProvider = CodeDomProvider.CreateProvider("c#");
CodeDomProvider cSharpProvider = new Microsoft.CSharp.CSharpCodeProvider();
```

## Important CodeDomProvider Methods

Static methods:

| Method                        | Description                                                                                             |
| ----------------------------- | ------------------------------------------------------------------------------------------------------- |
| `GetCompilerInfo`             | Returns CompilerInfo for a specific compiler configuration based on the provided language.              |
| `GetLanguageFromExtension`    | Returns the programming language that is associated with a specific file name extension.                |
| `IsDefinedExtension`          | Determines whether a specific file name extension is defined in the configuration settings.             |
| `IsDefinedLanguage`           | Determines whether a specific language name is defined in the configuration settings.                   |

Instance methods:

| Method                        | Description                                                                                   |
| ----------------------------- | --------------------------------------------------------------------------------------------- |
| `CompileAssemblyFromDom`      | Compiles an assembly from a CodeDOM compilation unit, producing an executable or a DLL.       |
| `CompileAssemblyFromFile`     | Compiles an assembly from a file containing source code, producing an executable or a DLL.    |
| `CompileAssemblyFromSource`   | Compiles an assembly from a string containing source code, producing an executable or a DLL.  |
| `GenerateCodeFromCompileUnit` | Generates code for a specified CodeCompileUnit and sends it to the specified text writer.     |
| `GenerateCodeFromNamespace`   | Generates code for a specified CodeNamespace and sends it to the specified text writer.       |
| `GenerateCodeFromType`        | Generates code for a specified CodeTypeDeclaration and sends it to the specified text writer. |

## Compiler parameters

```csharp
string sourceCode = /*...*/;

var providerOptions = new Dictionary<string, string>
{
	{ "CompilerVersion", "v11.0" }
};

CodeDomProvider provider = new Microsoft.CSharp.CSharpCodeProvider(providerOptions);

var compilerParameters = new string[] { /*...*/ };
CompilerResults results = provider.CompileAssemblyFromSource(compilerParameters), sourceCode);
```

## Compiler Features

The compiler declares a list of supported *Compiler Features* using the [GeneratorSupport](https://learn.microsoft.com/en-us/dotnet/api/system.codedom.compiler.generatorsupport) flags:
```csharp
var languageOptions = provider.LanguageOptions;
```

Those flags indicate if a given CodeDOM element is supported.

**Note** Compiling a CodeDOM code tree with elements that are not supported by the used compiler will result in an exception.

## See also

- [[Custom CodeDOM Providers]]