Used to suppress an `DiagnosticResult` under a given condition.

```csharp
[DiagnosticAnalyzer(LanguageNames.CSharp)]
public sealed class MySuppressor : DiagnosticSuppressor
{
	private static readonly SuppressionDescriptor RuleBenchmarkDotNet = new(
        id: "MAS0001",
        suppressedDiagnosticId: "CA1822",
        justification: "Suppress CA1822 on methods decorated with BenchmarkDotNet attributes."
    );

    public override ImmutableArray<SuppressionDescriptor> SupportedSuppressions => [RuleBenchmarkDotNet];

    public override void ReportSuppressions(SuppressionAnalysisContext context)
    {
        foreach (var diagnostic in context.ReportedDiagnostics)
		{
			ProcessDiagnostic(context, diagnostic);
		}
    }

    private static void ProcessDiagnostic(
	    SuppressionAnalysisContext context,
	    Diagnostic diagnostic)
    {
        var node = diagnostic.TryFindNode(context.CancellationToken);
        if (node is null)
            return;

        var semanticModel = context.GetSemanticModel(node.SyntaxTree);
        var symbol = semanticModel.GetDeclaredSymbol(node, context.CancellationToken);
        if (symbol is null)
            return;

        // ...

        var desciptor = RuleBenchmarkDotNet;
        var suppression = Suppression.Create(RuleBenchmarkDotNet, diagnostic);
        context.ReportSuppression(suppression);
    }
}
```
