## Code fix provider

- derive from `CodeFixProvider`
- override the `FixableDiagnosticIds` property to specify which diagnostics the code fix provider can fix
- everride the `RegisterCodeFixesAsync` method to provide the actual code fixes for the reported diagnostics

## Example: fixing non-specialized classes

```csharp
using System.Collections.Immutable;
using System.Composition;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CodeActions;
using Microsoft.CodeAnalysis.CodeFixes;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.CSharp.Syntax;

[ExportCodeFixProvider(LanguageNames.CSharp, Name = nameof(NonSpecializedClassCodeFixProvider)), Shared]
public class NonSpecializedClassCodeFixProvider : CodeFixProvider
{
    public sealed override ImmutableArray<string> FixableDiagnosticIds
        => ImmutableArray.Create(NonSpecializedClassAnalyzer.DiagnosticId);

    public sealed override FixAllProvider GetFixAllProvider() => WellKnownFixAllProviders.BatchFixer;

    public sealed override async Task RegisterCodeFixesAsync(CodeFixContext context)
    {
        var root = await context.Document.GetSyntaxRootAsync(context.CancellationToken).ConfigureAwait(false);

        // Find the class declaration identified by the diagnostic.
        var diagnostic = context.Diagnostics.First();
        var diagnosticSpan = diagnostic.Location.SourceSpan;
        var declaration = root.FindToken(diagnosticSpan.Start).Parent.AncestorsAndSelf().OfType<ClassDeclarationSyntax>().First();

        // Register a code action that will add the "static", "sealed", or "abstract" modifier to the class.
        context.RegisterCodeFix(
            CodeAction.Create(
                title: "Mark class as static",
                createChangedDocument: c => AddModifierAsync(context.Document, declaration, SyntaxKind.StaticKeyword, c),
                equivalenceKey: "Mark class as static"),
            diagnostic);

        context.RegisterCodeFix(
            CodeAction.Create(
                title: "Mark class as sealed",
                createChangedDocument: c => AddModifierAsync(context.Document, declaration, SyntaxKind.SealedKeyword, c),
                equivalenceKey: "Mark class as sealed"),
            diagnostic);

        context.RegisterCodeFix(
            CodeAction.Create(
                title: "Mark class as abstract",
                createChangedDocument: c => AddModifierAsync(context.Document, declaration, SyntaxKind.AbstractKeyword, c),
                equivalenceKey: "Mark class as abstract"),
            diagnostic);
    }

    private async Task<Document> AddModifierAsync(
	    Document document,
	    ClassDeclarationSyntax classDeclaration,
	    SyntaxKind modifierKind,
	    CancellationToken cancellationToken)
    {
        var modifier = SyntaxFactory.Token(modifierKind);
        var newNode = classDeclaration.AddModifiers(modifier);

        var root = await document.GetSyntaxRootAsync(cancellationToken);
        var newRoot = root.ReplaceNode(classDeclaration, newNode);

        return document.WithSyntaxRoot(newRoot);
    }
}
```
