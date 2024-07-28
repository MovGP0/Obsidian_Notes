## Install NuGet packages

```powershell
dotnet add package Microsoft.NET.Test.Sdk
dotnet add package xunit
dotnet add package xunit.runner.visualstudio
dotnet add package Microsoft.CodeAnalysis.CSharp.Analyzer.Testing
dotnet add package Microsoft.CodeAnalysis.CSharp.CodeFix.Testing
dotnet add package Microsoft.CodeAnalysis.CSharp.Workspaces
```

## Test with analyzer

```csharp
var context = new CSharpAnalyzerTest<MyAnalyzer, DefaultVerifier>();
context.ReferenceAssemblies = ReferenceAssemblies.Net.Net80;

// The code between [| and |] is the code that should produce the diagnostic.
context.TestCode = """
        class [|Type1|] { }
        """;

await context.RunAsync();
```

## Test with analyzer and code fix provider

```csharp
var context = new CSharpCodeFixTest<MyAnalyzer, MyAnalyzerCodeFixProvider, DefaultVerifier>();
context.ReferenceAssemblies = ReferenceAssemblies.Net.Net80;

context.TestCode = """
	class [|Type1|] { }
	class [|Type2|] { }
	""";

context.FixedCode = """
	class TYPE1 { }
	class TYPE2 { }
	""";

await context.RunAsync();
```

### Execute specific code fix provider

Either declare a `CodeActionIndex`
```csharp
context.CodeActionIndex = 1;
```

or the `CodeActionEquivalenceKey`
```csharp
context.CodeActionEquivalenceKey = "key";
```

Only the specified code fix will be executed.

## Verification

### Verify with `DiagnosticResult`

```csharp
var expected = DiagnosticResult  
    .CompilerWarning(DiagnosticId)  
    .WithSpan(6, 22, 6, 29)  
    .WithArguments("");

context.ExpectedDiagnostics.Add(expected);
```

### Verify with marker syntax

Code with diagnostic location can be placed between `[|` and `|]`
```
[|CODE_WITH_DIAGNOSTIC|]
```

The diagnostic ID can be specified with the `DiagnosticId`
```
{|DiagnosticId:CODE_WITH_DIAGNOSTIC|}
```

An `Location`-Index (`{|#INDEX:...|}`) can be used for referencing a specific `DiagnosticResult`:
```
{|#0:CODE_WITH_DIAGNOSTIC|}
```
```csharp
context.ExpectedDiagnostics.Add(new DiagnosticResult(DiagnosticId).WithLocation(0).WithArguments("Type1"));
```

## Adding additional source files

```csharp
context.Sources.Add("file1.cs", """
    global using System;
    """);
```

## Adding reference assemblies

Default reference assemblies
```csharp
context.ReferenceAssemblies = ReferenceAssemblies.Net.Net80;
context.ReferenceAssemblies = ReferenceAssemblies.Net.Net80Windows;
context.ReferenceAssemblies = ReferenceAssemblies.NetStandard.NetStandard20;
context.ReferenceAssemblies = ReferenceAssemblies.NetFramework.Net48.Wpf;
```

Custom reference assemblies
```csharp
var refAssemblyId = new PackageIdentity("Microsoft.NETCore.App.Ref", "9.0.0-preview.3.24172.9");
var refAssemblies = new ReferenceAssemblies(
        targetFramework: "net9.0",
        referenceAssemblyPackage: refAssemblyId,
        referenceAssemblyPath: Path.Combine("ref", "net9.0"));

context.ReferenceAssemblies = refAssemblies;
```

## Adding NuGet packages

```csharp
context.ReferenceAssemblies = ReferenceAssemblies.Net.Net80
    .AddPackages([
        new PackageIdentity("Microsoft.AspNetCore.App.Ref", "8.0.4"),
        new PackageIdentity("Microsoft.WindowsDesktop.App.Ref", "8.0.4"),
        new PackageIdentity("Microsoft.Windows.SDK.NET.Ref", "10.0.22621.33"),
        new PackageIdentity("Meziantou.Framework.FullPath", "1.0.12")
    ]);
```

## Adding additional files

```csharp
context.TestState.AdditionalFiles.Add(("sample.txt", "content"));
```

## Adding `.editorconfig` files

```csharp
context.TestState.AnalyzerConfigFiles.Add(("/sample.editorconfig", "name = value"));
```

## Defining assembly output kind

Create DLL
```csharp
context.TestState.OutputKind = OutputKind.DynamicallyLinkedLibrary;
```

Create a console application
```csharp
context.TestState.OutputKind = OutputKind.ConsoleApplication;
```

Create a Windows application
```csharp
context.TestState.OutputKind = OutputKind.WindowsApplication
```

## Setting parser options

```csharp
var options = new CSharpParseOptions(
        languageVersion: LanguageVersion.CSharp4,
        preprocessorSymbols: ["DEBUG"]);

context.SolutionTransforms.Add((solution, projectId) =>
{
    return solution.WithProjectParseOptions(projectId, options);
});
```

## Configuring compiler diagnostics

All diagnostics
```csharp
context.CompilerDiagnostics = CompilerDiagnostics.All;
```

Errors
```csharp
context.CompilerDiagnostics = CompilerDiagnostics.Errors;
```

Warnings and errors
```csharp
context.CompilerDiagnostics = CompilerDiagnostics.Warnings;
```

Suggestions, warnings and errors
```csharp
context.CompilerDiagnostics = CompilerDiagnostics.Suggestions;
```

## Disable diagnostics

```csharp
context.CompilerDiagnostics = CompilerDiagnostics.All;
context.DisabledDiagnostics.Add("CS8019"); // Unused using directives
```

## Alternative Verifier definition

A `using` statement can be used to declare the verifier
```csharp
using Verifier = Microsoft.CodeAnalysis.CSharp.Testing.CSharpAnalyzerVerifier<MyAnalyzer, Microsoft.CodeAnalysis.Testing.DefaultVerifier>;
```

## References

* [How to test a Roslyn analyzer](https://www.meziantou.net/how-to-test-a-roslyn-analyzer.htm)