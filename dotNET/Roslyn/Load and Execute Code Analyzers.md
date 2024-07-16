
Load code analyzers
```csharp
private static ImmutableArray<DiagnosticAnalyzer> LoadAnalyzers()
{
	// Load analyzer assemblies
	var analyzerPaths = new[]
	{
		// Path to the FxCop analyzers DLL or any other analyzer
		Path.Combine(Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location), "Microsoft.CodeAnalysis.FxCopAnalyzers.dll")
	};

	var analyzers = new List<DiagnosticAnalyzer>();

	foreach (var path in analyzerPaths)
	{
		var assembly = Assembly.LoadFrom(path);
		analyzers.AddRange(assembly.GetTypes()
			.Where(t => typeof(DiagnosticAnalyzer).IsAssignableFrom(t) && !t.IsAbstract)
			.Select(t => (DiagnosticAnalyzer)Activator.CreateInstance(t)!));
	}

	return analyzers.ToImmutableArray();
}
```

Configure analyzers
```csharp
var analyzerOptions = new CompilationWithAnalyzersOptions(
            new AnalyzerOptions(ImmutableArray<AdditionalText>.Empty),
            onAnalyzerException: null,
            concurrentAnalysis: true,
            logAnalyzerExecutionTime: false,
            reportSuppressedDiagnostics: false,
            ReportDiagnostic.Error)
```

Add analyzers to compilation
```csharp
var compilation = CSharpCompilation
    .Create("DynamicCompilation")
	.WithOptions(compileOptions)
	.AddReferences(references)
	.AddSyntaxTrees(syntaxTree)
	.WithAnalyzers(LoadAnalyzers(), analyzerOptions);
```

Analyze code
```csharp
var diagnostics = compilationWithAnalyzers.GetAnalyzerDiagnosticsAsync().Result;

foreach (var diagnostic in diagnostics)
{
	Console.WriteLine(diagnostic.ToString());
}
```
