## Reference external libraries

```csharp
#r "..\SomeLibrary.dll" // reference external library
#load "SomeOtherQuery.linq" // import other .linq file

#region private::tests
// code placed here is for unit testing
// it is not exported when the file is referenced using #load
#endregion
```

## Customize output format

### Implementing `ToDump` methods

See [LinqPad Manual: Customizing Dump](https://www.linqpad.net/CustomizingDump.aspx) for details.

### Render with specific format
```csharp
Util.WithStyle(objectToDump, "font-family:consolas; color: red;").Dump(heading, depth);
```

### Render as Markdown

Render output as formatted Markdown
```csharp
string markdown = "...";

Util.HtmlHead.AddScriptFromUri("https://code.jquery.com/jquery-3.4.1.min.js");
Util.RawHtml($"<md>{markdown}</md>").Dump();
```

## Render with syntax highlighting

```csharp
Util.SyntaxColorText(csharp, SyntaxLanguageStyle.CSharp).Dump(); // Dump inline
Util.SyntaxColorText(json, SyntaxLanguageStyle.Json).DumpToNewPanel(); // Dump to separate panel
```
