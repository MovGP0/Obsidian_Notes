## Install NuGet packages

```bash
dotnet add package Accord
dotnet add package Accord.Imaging
dotnet add package Accord.MachineLeaning
```

## Add Namespaces

```cs
using Accord.Imaging;
using Accord.MachineLearning;
using System.Drawing;
using System.Drawing.Imaging;
using System.Runtime.InteropServices;
```

## Get the Desktop Wallpaper

```cs
// Define the PInvoke signature
public static class NativeMethods
{
    [DllImport("user32.dll", CharSet = CharSet.Auto)]
    public static extern int SystemParametersInfo(int uAction, int uParam, StringBuilder lpvParam, int fuWinIni);
    public const int SPI_GETDESKWALLPAPER = 0x0073;
}

// Function to get the current desktop wallpaper path
string GetDesktopWallpaper()
{
    StringBuilder wallpaper = new StringBuilder(260);
    NativeMethods.SystemParametersInfo(NativeMethods.SPI_GETDESKWALLPAPER, wallpaper.Capacity, wallpaper, 0);
    return wallpaper.ToString();
}
```

**See also**
- [User32.SystemParametersInfo](https://www.pinvoke.net/default.aspx/user32.systemparametersinfo)
- [SPI Enumeration](https://pinvoke.net/default.aspx/Enums/SPI.html)

## Extract primary colors from image
```cs
Color[] ExtractPrimaryColors(string imagePath, int nColors = 5)
{
    // Load the image
    using Bitmap bitmap = new Bitmap(imagePath);

    // Resize the image to reduce processing time
    using Bitmap resizedBitmap = new Bitmap(bitmap, new Size(100, 100));

    // Extract pixels from the image
    int width = resizedBitmap.Width;
    int height = resizedBitmap.Height;
    var pixels = new List<double[]>();

    for (int y = 0; y < height; y++)
    {
        for (int x = 0; x < width; x++)
        {
            Color color = resizedBitmap.GetPixel(x, y);
            pixels.Add(new double[] { color.R, color.G, color.B });
        }
    }

    double[][] pixelArray = pixels.ToArray();

    // Apply KMeans clustering
    KMeans kmeans = new KMeans(nColors);
    int[] labels = kmeans.Learn(pixels.ToArray()).Decide(pixels.ToArray());

    // Get the primary colors
    var primaryColors = kmeans.Clusters.Centroids
        .Select(c => Color.FromArgb((int)c[0], (int)c[1], (int)c[2]))
        .ToArray();

    return primaryColors;
}
```

## HSL ⇆ RGB conversion methods

```cs
(double H, double S, double L) RgbToHsl(Color color)
{
    double r = color.R / 255.0;
    double g = color.G / 255.0;
    double b = color.B / 255.0;

    double max = Math.Max(r, Math.Max(g, b));
    double min = Math.Min(r, Math.Min(g, b));
    double delta = max - min;

    double h = 0;
    if (delta == 0)
        h = 0;
    else if (max == r)
        h = 60 * (((g - b) / delta) % 6);
    else if (max == g)
        h = 60 * (((b - r) / delta) + 2);
    else if (max == b)
        h = 60 * (((r - g) / delta) + 4);

    if (h < 0) h += 360;

    double l = (max + min) / 2;
    double s = (delta == 0) ? 0 : delta / (1 - Math.Abs(2 * l - 1));

    return (h, s, l);
}
```

```cs
Color HslToRgb(double h, double s, double l)
{
    double c = (1 - Math.Abs(2 * l - 1)) * s;
    double x = c * (1 - Math.Abs((h / 60) % 2 - 1));
    double m = l - c / 2;

    double r = 0, g = 0, b = 0;

    if (0 <= h && h < 60)
    {
        r = c;
        g = x;
        b = 0;
    }
    else if (60 <= h && h < 120)
    {
        r = x;
        g = c;
        b = 0;
    }
    else if (120 <= h && h < 180)
    {
        r = 0;
        g = c;
        b = x;
    }
    else if (180 <= h && h < 240)
    {
        r = 0;
        g = x;
        b = c;
    }
    else if (240 <= h && h < 300)
    {
        r = x;
        g = 0;
        b = c;
    }
    else if (300 <= h && h < 360)
    {
        r = c;
        g = 0;
        b = x;
    }

    r = (r + m) * 255;
    g = (g + m) * 255;
    b = (b + m) * 255;

    return Color.FromArgb((int)r, (int)g, (int)b);
}
```

## Calculate color complements

```cs
Color GetRgbComplement(Color color)
{
    return Color.FromArgb(255 - color.R, 255 - color.G, 255 - color.B);
}
```

```cs
Color GetHslComplement(Color color)
{
    var (h, s, l) = RgbToHsl(color);
    h = (h + 180) % 360;
    return HslToRgb(h, s, l);
}
```

## Methods to create additional colors

Blend existing colors to create new colors.

```cs
Color[] BlendColors(Color[] colors, int nColors)
{
    var blendedColors = new List<Color>(colors);

    while (blendedColors.Count < nColors)
    {
        for (int i = 0; i < colors.Length - 1; i++)
        {
            if (blendedColors.Count >= nColors) break;
            var color1 = colors[i];
            var color2 = colors[i + 1];
            var blendedColor = Color.FromArgb(
                (color1.R + color2.R) / 2,
                (color1.G + color2.G) / 2,
                (color1.B + color2.B) / 2
            );
            blendedColors.Add(blendedColor);
        }
    }

    return blendedColors.ToArray();
}

```

Interpolate between existing colors to create a new color.

```cs
// Function to extrapolate colors if there are fewer than required
Color[] ExtrapolateColors(Color[] colors, int nColors)
{
    var extendedColors = new List<Color>(colors);
    Random random = new Random();

    while (extendedColors.Count < nColors)
    {
        foreach (var color in colors)
        {
            if (extendedColors.Count >= nColors) break;
            var extrapolatedColor = Color.FromArgb(
                Clamp(color.R + random.Next(-10, 11)),
                Clamp(color.G + random.Next(-10, 11)),
                Clamp(color.B + random.Next(-10, 11))
            );
            extendedColors.Add(extrapolatedColor);
        }
    }

    return extendedColors.ToArray();

    // Helper function to clamp color values between 0 and 255
    int Clamp(int value)
    {
        return Math.Max(0, Math.Min(255, value));
    }
}
```

**Note:** if there are not enough colors for extrapolation/blending, add neutral colors like black and white.

### Extrapolate additional colors from a single color

```cs
Color[] GenerateAnalogousColors(Color baseColor, int nColors)
{
    var (h, s, l) = RgbToHsl(baseColor);
    var colors = new List<Color> { baseColor };

    for (int i = 1; i <= nColors / 2; i++)
    {
        colors.Add(HslToRgb((h + i * 30) % 360, s, l));
        colors.Add(HslToRgb((h - i * 30 + 360) % 360, s, l));
    }

    return colors.Take(nColors).ToArray();
}
```

```cs
Color[] GenerateMonochromaticColors(Color baseColor, int nColors)
{
    var (h, s, l) = RgbToHsl(baseColor);
    var colors = new List<Color> { baseColor };

    for (int i = 1; i < nColors; i++)
    {
        double factor = i / (double)nColors;
        colors.Add(HslToRgb(h, s, l * factor));
        colors.Add(HslToRgb(h, s, l + (1 - l) * factor));
    }

    return colors.Take(nColors).ToArray();
}
```

```cs
Color[] GenerateTriadColors(Color baseColor, int nColors)
{
    var (h, s, l) = RgbToHsl(baseColor);
    return new Color[]
    {
        baseColor,
        HslToRgb((h + 120) % 360, s, l),
        HslToRgb((h + 240) % 360, s, l)
    }.Take(nColors).ToArray();
}
```

```cs
Color[] GenerateComplementaryColors(Color baseColor, int nColors)
{
    var (h, s, l) = RgbToHsl(baseColor);
    return new Color[]
    {
        baseColor,
        HslToRgb((h + 180) % 360, s, l)
    }.Take(nColors).ToArray();
}
```

```cs
Color[] GenerateSplitComplementaryColors(Color baseColor, int nColors)
{
    var (h, s, l) = RgbToHsl(baseColor);
    return new Color[]
    {
        baseColor,
        HslToRgb((h + 150) % 360, s, l),
        HslToRgb((h - 150 + 360) % 360, s, l)
    }.Take(nColors).ToArray();
}
```

```cs
Color[] GenerateSquareColors(Color baseColor, int nColors)
{
    var (h, s, l) = RgbToHsl(baseColor);
    return new Color[]
    {
        baseColor,
        HslToRgb((h + 90) % 360, s, l),
        HslToRgb((h + 180) % 360, s, l),
        HslToRgb((h + 270) % 360, s, l)
    }.Take(nColors).ToArray();
}
```

```cs
Color[] GenerateCompoundColors(Color baseColor, int nColors)
{
    var (h, s, l) = RgbToHsl(baseColor);
    var complementary = (h + 180) % 360;
    return new Color[]
    {
        baseColor,
        HslToRgb((complementary + 30) % 360, s, l),
        HslToRgb((complementary - 30 + 360) % 360, s, l),
        HslToRgb((complementary + 60) % 360, s, l),
        HslToRgb((complementary - 60 + 360) % 360, s, l)
    }.Take(nColors).ToArray();
}
```

```cs
Color[] GenerateShades(Color baseColor, int nColors)
{
    var (h, s, l) = RgbToHsl(baseColor);
    var colors = new List<Color> { baseColor };

    for (int i = 1; i < nColors; i++)
    {
        double factor = i / (double)nColors;
        colors.Add(HslToRgb(h, s, l * (1 - factor)));
    }

    return colors.Take(nColors).ToArray();
}
```

## Example

```cs
// Function to create and display a color theme
Bitmap PlotColorTheme(Color[] colors)
{
    int width = 100;
    int height = 100;
    int barWidth = width / colors.Length;

    Bitmap bitmap = new Bitmap(width, height);

    using Graphics g = Graphics.FromImage(bitmap);

    for (int i = 0; i < colors.Length; i++)
    {
        using Brush brush = new SolidBrush(colors[i]);
        g.FillRectangle(brush, i * barWidth, 0, barWidth, height);
    }

    return bitmap;
}
```

```cs
string imagePath = GetDesktopWallpaper();
int requiredColors = 10;

Color[] primaryColors = ExtractPrimaryColors(imagePath, nColors: requiredColors);
var baseColor = primaryColors[0];

// Get complementary colors
Color[] rgbComplements = primaryColors.Select(GetRgbComplement).ToArray();
Color[] hslComplements = primaryColors.Select(GetHslComplement).ToArray();

// Display color themes
"Primary Colors:".Dump();
PlotColorTheme(primaryColors);

"RGB Complements:".Dump();
PlotColorTheme(rgbComplements);

"HSL Complements:".Dump();
PlotColorTheme(hslComplements);

"Analogous Colors:".Dump();
PlotColorTheme(GenerateAnalogousColors(baseColor, requiredColors));

"Monochromatic Colors:".Dump();
PlotColorTheme(GenerateMonochromaticColors(baseColor, requiredColors));

"Triad Colors:".Dump();
PlotColorTheme(GenerateTriadColors(baseColor, requiredColors));

"Complementary Colors:".Dump();
PlotColorTheme(GenerateComplementaryColors(baseColor, requiredColors));

"Split Complementary Colors:".Dump();
PlotColorTheme(GenerateSplitComplementaryColors(baseColor, requiredColors));

"Square Colors:".Dump();
PlotColorTheme(GenerateSquareColors(baseColor, requiredColors));

"Compound Colors:".Dump();
PlotColorTheme(GenerateCompoundColors(baseColor, requiredColors));

"Shades:".Dump();
PlotColorTheme(GenerateShades(baseColor, requiredColors));
```

## Windows theme and colors

Check if light or dark theme is enabled
```cs
public static WindowsTheme GetWindowsTheme()
{
    const string registryKeyPath = @"Software\Microsoft\Windows\CurrentVersion\Themes\Personalize";
    const string registryValueName = "AppsUseLightTheme";

    try
    {
        using RegistryKey key = Registry.CurrentUser.OpenSubKey(registryKeyPath);

        if (key != null)
        {
            object registryValueObject = key.GetValue(registryValueName);
            if (registryValueObject != null)
            {
                int registryValue = (int)registryValueObject;

                return registryValue == 0
                    ? WindowsTheme.Dark
                    : WindowsTheme.Light;
            }
        }
    }
    catch (Exception ex)
    {
        // Handle the exception as needed
        Console.WriteLine("Error reading registry: " + ex.Message);
    }

    return WindowsTheme.Unknown;
}

public enum WindowsTheme
{
    Light,
    Dark,
    Unknown
}
```

Get windows theme colors
```bash
dotnet add package Microsoft.Windows.Compatibility
```

```cs
using Windows.UI.ViewManagement;
```

```cs
// Initialize UISettings
var uiSettings = new UISettings();

// Get the accent color
var accentColor = uiSettings.GetColorValue(UIColorType.Accent);

// Get the background color
var backgroundColor = uiSettings.GetColorValue(UIColorType.Background);

// Get the foreground color
var foregroundColor = uiSettings.GetColorValue(UIColorType.Foreground);

// Convert colors from Windows.UI.Color to System.Windows.Media.Color
var accent = Color.FromArgb(accentColor.A, accentColor.R, accentColor.G, accentColor.B);
var background = Color.FromArgb(backgroundColor.A, backgroundColor.R, backgroundColor.G, backgroundColor.B);
var foreground = Color.FromArgb(foregroundColor.A, foregroundColor.R, foregroundColor.G, foregroundColor.B);
```