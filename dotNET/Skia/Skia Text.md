Declare a font
```cs
using var typeface = SKTypeface.FromFamilyName(
	familyName: "Impact",
	weight: SKFontStyleWeight.SemiBold,
	width: SKFontStyleWidth.Normal,
	slant: SKFontStyleSlant.Italic);

using var paint = new SKPaint()
{
    Color = SKColors.Yellow,
    IsAntialias = true,
    TextSize = 64,
    Typeface = typeface
};
```

## Measure width

`SKPaint.MeasureText` returns the width of a given text
```cs
float width = paint.MeasureText("Foobar");
```

## Font metrics for positioning

```cs
// Retrieve font metrics
SKFontMetrics metrics = paint.FontMetrics;

// Access various metrics
float ascent = metrics.Ascent;
float descent = metrics.Descent;
float leading = metrics.Leading;
float xHeight = metrics.XHeight;
float capHeight = metrics.CapHeight;
float underlinePosition = metrics.UnderlinePosition;
float underlineThickness = metrics.UnderlineThickness;
```

| Property              | Description                                                              |
| --------------------- | ------------------------------------------------------------------------ |
| Ascent                | recommended distance above the baseline.                                 |
| AverageCharacterWidth | average character width.                                                 |
| Bottom                | greatest distance below the baseline for any glyph.                      |
| CapHeight             | cap height.                                                              |
| Descent               | recommended distance below the baseline.                                 |
| Leading               | recommended distance to add between lines of text.                       |
| MaxCharacterWidth     | max character width.                                                     |
| StrikeoutPosition     | position of the bottom of the strikeout stroke relative to the baseline. |
| StrikeoutThickness    | thickness of the strikeout.                                              |
| Top                   | greatest distance above the baseline for any glyph.                      |
| UnderlinePosition     | position of the top of the underline stroke relative to the baseline.    |
| UnderlineThickness    | thickness of the underline.                                              |
| XHeight               | height of an 'x' in px.                                                  |
| XMax                  | maximum bounding box x value for all glyphs.                             |
| XMin                  | minimum bounding box x value for all glyphs.                             |

![[Attachements/Pasted image 20241103124716.png]]

## Create typeface from embedded font

```csharp
var assembly = Assembly.GetExecutingAssembly();
var resourceName = "YourNamespace.MyFont.ttf";

using (var stream = assembly.GetManifestResourceStream(resourceName))
{
	if (stream != null)
	{
		return SKTypeface.FromStream(stream);
	}
	else
	{
		throw new FileNotFoundException("Font 'MyFont.ttf' not found in resources.");
	}
}
```

## Create typeface from `*.ttf` file

```cs
using var typeface = SKTypeface.FromFile("/path/to/MyFont.ttf");
```