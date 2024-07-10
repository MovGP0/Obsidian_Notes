
Get the source code
```csharp
string sourceCode = richTextBox.Text;
```

Parse the code to create the CodeDOM
```

```

Apply the formatting to the `RichTextBox`
```csharp
private static readonly Color KeywordColor = Color.Blue;
private static readonly Color StringColor = Color.Brown;
private static readonly Color CommentColor = Color.Green;

public static void HighlightSyntax(RichTextBox richTextBox, string sourceCode)
{
	var tree = CSharpSyntaxTree.ParseText(sourceCode);
	var root = tree.GetRoot();
	
	var textSpans = new List<(TextSpan, Color)>();
	
	HighlightNodes(root, textSpans);

	ApplyFormatting(richTextBox, textSpans);
}

private static void HighlightNodes(SyntaxNode node, List<(TextSpan, Color)> textSpans)
{
	foreach (var token in node.DescendantTokens())
	{
		switch (token.Kind())
		{
			case SyntaxKind.StringLiteralToken:
				textSpans.Add((token.Span, StringColor));
				break;

			case SyntaxKind.SingleLineCommentTrivia:
			case SyntaxKind.MultiLineCommentTrivia:
				textSpans.Add((token.Span, CommentColor));
				break;

			default:
				if (token.IsKeyword())
				{
					textSpans.Add((token.Span, KeywordColor));
				}
				break;
		}
	}
}

private static void ApplyFormatting(RichTextBox richTextBox, List<(TextSpan, Color)> textSpans)
{
	richTextBox.Text = richTextBox.Text; // Reset the text to clear previous formatting
	foreach (var (span, color) in textSpans)
	{
		richTextBox.Select(span.Start, span.Length);
		richTextBox.SelectionColor = color;
	}

	richTextBox.DeselectAll();
}
```