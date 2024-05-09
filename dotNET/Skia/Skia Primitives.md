## Create a bitmap and canvas to paint on
```cs
using var bitmap = new SKBitmap(width, height);
using var canvas = new SKCanvas(bitmap);
canvas.Clear(SKColors.White);
```

## Create a paint
```cs
using var paint = new SKPaint();
paint.Color = SKColors.Red;
paint.TextSize = 12.0f;
paint.IsAntialias = true;
paint.Color = SKColors.Red;
paint.IsStroke = false;
paint.TextAlign = SKTextAlign.Center;
```

## Draw text

Measure the size of text
```cs
var bounds = new SKRect();
paint.MeasureText(TEXT, ref bounds);
```

Draw the text
```cs
canvas.DrawText("foobar", posX, posY, paint);
```

## Draw Polygons

```cs
using var path = new SKPath();
path.MoveTo(380, 390);
path.CubicTo(560, 390, 560, 280, 500, 280);
// ...

canvas.DrawPath(path, paint);
```

## Draw Bitmap

```cs
canvas.DrawBitmap(originalBitmap,
    new SKRect(112, 238, 184, 310), // source
    new SKRect(0, 0, 9, 9)); // destination
```

## Translate/Rotate Canvas

```cs
canvas.RotateDegrees(90.0f/*degrees*/, posX, posY);
canvas.RotateRadians((float)Math.PI / 2.0f/*radians*/, posX, posY);
canvas.Translate(dx, dy);
```

## Save Image to File
```cs
using var stream = new FileStream(outputPath, FileMode.Create, FileAccess.Write);
using var image = SKImage.FromBitmap(bitmap);
using var encodedImage = image.Encode();
encodedImage.SaveTo(stream);
```

## See also

- [SKCanvas Methods](https://learn.microsoft.com/en-us/dotnet/api/skiasharp.skcanvas#methods)