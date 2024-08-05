## Implement Analyzer

- derive from `Microsoft.CodeAnalysis.Diagnostics.DiagnosticAnalyzer`
- override the `SupportedDiagnostics` property to specify the diagnostics the analyzer will report
- override the `Initialize` method to register the analyzer's actions
    - you'll typically register a callback for `SyntaxKind` or `SymbolKind` using `context.RegisterSyntaxNodeAction` or `context.RegisterSymbolAction` methods. 
    - The registered callback will receive a context object containing the semantic model, which you can use to perform your analysis.
- implement diagnostics reporting
	 - In the registered callback, use the context object to access the semantic model and perform your analysis.
	 - If you find an issue, report a diagnostic using the `context.ReportDiagnostic` method.

## Example: find unused variables

```csharp
using System.Collections.Immutable;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.CSharp.Syntax;
using Microsoft.CodeAnalysis.Diagnostics;

[DiagnosticAnalyzer(LanguageNames.CSharp)]
public class UnusedLocalVariableAnalyzer : DiagnosticAnalyzer
{
    public const string DiagnosticId = "UnusedLocalVariable";
    private static readonly LocalizableString Title = new LocalizableResourceString(nameof(Resources.UnusedLocalVariableTitle), Resources.ResourceManager, typeof(Resources));
    private static readonly LocalizableString MessageFormat = new LocalizableResourceString(nameof(Resources.UnusedLocalVariableMessage), Resources.ResourceManager, typeof(Resources));
    private static readonly LocalizableString Description = new LocalizableResourceString(nameof(Resources.UnusedLocalVariableDescription), Resources.ResourceManager, typeof(Resources));
    private const string Category = "Usage";

    private static readonly DiagnosticDescriptor Rule = new DiagnosticDescriptor(
        DiagnosticId,
        Title,
        MessageFormat,
        Category,
        DiagnosticSeverity.Warning,
        isEnabledByDefault: true,
        description: Description);

    public override ImmutableArray<DiagnosticDescriptor> SupportedDiagnostics => ImmutableArray.Create(Rule);

    public override void Initialize(AnalysisContext context)
    {
        context.ConfigureGeneratedCodeAnalysis(GeneratedCodeAnalysisFlags.None);
        context.EnableConcurrentExecution();
        context.RegisterSyntaxNodeAction(AnalyzeSyntaxNode, SyntaxKind.LocalDeclarationStatement);
    }

    private void AnalyzeSyntaxNode(SyntaxNodeAnalysisContext context)
    {
        var localDeclaration = (LocalDeclarationStatementSyntax)context.Node;

        foreach (var variable in localDeclaration.Declaration.Variables)
        {
            var symbol = context.SemanticModel.GetDeclaredSymbol(variable, context.CancellationToken);
            var dataFlow = context.SemanticModel.AnalyzeDataFlow(variable.Initializer.Value);

            if (!dataFlow.WrittenInside.Contains(symbol))
            {
                var diagnostic = Diagnostic.Create(Rule, variable.GetLocation(), variable.Identifier.Text);
                context.ReportDiagnostic(diagnostic);
            }
        }
    }
}
```

## Example: find non-specialized classes

```csharp
using System.Collections.Immutable;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.Diagnostics;

[DiagnosticAnalyzer(LanguageNames.CSharp)]
public class NonSpecializedClassAnalyzer : DiagnosticAnalyzer
{
    public const string DiagnosticId = "NonSpecializedClass";
    private static readonly LocalizableString Title = new LocalizableResourceString(nameof(Resources.NonSpecializedClassTitle), Resources.ResourceManager, typeof(Resources));
    private static readonly LocalizableString MessageFormat = new LocalizableResourceString(nameof(Resources.NonSpecializedClassMessage), Resources.ResourceManager, typeof(Resources));
    private static readonly LocalizableString Description = new LocalizableResourceString(nameof(Resources.NonSpecializedClassDescription), Resources.ResourceManager, typeof(Resources));
    private const string Category = "Design";

    private static readonly DiagnosticDescriptor Rule = new DiagnosticDescriptor(
        DiagnosticId,
        Title,
        MessageFormat,
        Category,
        DiagnosticSeverity.Warning,
        isEnabledByDefault: true,
        description: Description);

    public override ImmutableArray<DiagnosticDescriptor> SupportedDiagnostics => ImmutableArray.Create(Rule);

    public override void Initialize(AnalysisContext context)
    {
        context.ConfigureGeneratedCodeAnalysis(GeneratedCodeAnalysisFlags.None);
        context.EnableConcurrentExecution();
        context.RegisterSymbolAction(AnalyzeSymbol, SymbolKind.NamedType);
    }

    private void AnalyzeSymbol(SymbolAnalysisContext context)
    {
        var namedTypeSymbol = (INamedTypeSymbol)context.Symbol;

        // Check if the symbol is a class, and not static, sealed, or abstract
        if (namedTypeSymbol.TypeKind == TypeKind.Class && !namedTypeSymbol.IsStatic && !namedTypeSymbol.IsSealed && !namedTypeSymbol.IsAbstract)
        {
            var diagnostic = Diagnostic.Create(Rule, namedTypeSymbol.Locations[0], namedTypeSymbol.Name);
            context.ReportDiagnostic(diagnostic);
        }
    }
}
```
