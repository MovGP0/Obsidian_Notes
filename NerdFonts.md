## Nerd Fonts

https://www.nerdfonts.com/font-downloads

| Windows Font      | Nerd Font Replacement           |
| ----------------- | ------------------------------- |
| Arial             | Arimo                           |
| Times New Roman   | Tinos                           |
| Courier New       | Cousine, AnonymicePro           |
| Verdana           | DejaVuSansM                     |
| Georgia           |                                 |
| Tahoma            | Arimo, BitstromWera, Cousine    |
| Calibri           |                                 |
| Consolas          | Inconsolata                     |
| Segoe UI          |                                 |
| Cambria           |                                 |
| Lucida Console    |                                 |
| Comic Sans MS     | ComicShannsMono                 |
| Palatino Linotype |                                 |
| Trebuchet MS      |                                 |
| Century Gothic    |                                 |
| Cascadia Code     | Cascadia Code NF, CaskaydiaCove |
| Cascadia Mono     | Cascadia Mono NF, CaskaydiaMono |

## Package Font with Application

1. Add a `/Fonts` folder to your solution. 
2. Add the True Type Fonts (`*.ttf`) files to that folder 
3. Include the files to the project
4. Select the fonts and add them to the solution
5. Set `BuildAction: Resource` and `Copy To Output Directory: Do not copy`. Your `.csproj` file should now have a section like this one: 
```xml
<ItemGroup>
    <Resource Include="Fonts\NotoSans-Bold.ttf" />
    <Resource Include="Fonts\NotoSans-BoldItalic.ttf" />
    <Resource Include="Fonts\NotoSans-Italic.ttf" />
    <Resource Include="Fonts\NotoSans-Regular.ttf" />
    <Resource Include="Fonts\NotoSansSymbols-Regular.ttf" />
</ItemGroup>
```

## Copy Font to Output Directory

1. Add a `/Fonts` folder to your solution. 
2. Add the True Type Fonts (`*.ttf`) files to that order 
3. Include the files to the project
4. Select the fonts and add them to the solution
5. Set `BuildAction: Content` and `Copy To Output Directory: Copy if newer` or `Copy always`. Your `.csproj` file should now have a section like this one: 
```xml
<ItemGroup>
	<Content Include="Fonts\NotoSans-Bold.ttf">
		<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
	</Content>
	<Content Include="Fonts\NotoSans-BoldItalic.ttf">
		<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
	</Content>
	<Content Include="Fonts\NotoSans-Italic.ttf">
		<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
	</Content>
	<Content Include="Fonts\NotoSans-Regular.ttf">
		<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
	</Content>
	<Content Include="Fonts\NotoSansSymbols-Regular.ttf">
		<CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
	</Content>
</ItemGroup>
```

## WPF: Load Packaged Font

In `App.xaml` add `<FontFamily>` resources. It should look like in the following code sample. Note that the URI doesn't contain the filename when packing with the application. 
```xml
<Applicaton ...>
	<Application.Resources>
	    <FontFamily x:Key="NotoSans">pack://application:,,,/Fonts/#Noto Sans</FontFamily>
        <FontFamily x:Key="NotoSansSymbols">pack://application:,,,/Fonts/#Noto Sans Symbols</FontFamily>
    </Application.Resources>
</Application>
```

Apply your fonts like this: 
```xml
<TextBlock x:Name="myTextBlock" Text="foobar" FontFamily="{StaticResource NotoSans}" 
           FontSize="10.0" FontStyle="Normal" FontWeight="Regular" />
```

You can also set the font imperatively: 
```csharp
myTextBlock.FontFamily = new FontFamily(new Uri("pack://application:,,,/"), "./Fonts/#Noto Sans");
```

### WPF: Load Font from Output Directory

- In `App.xaml` add `<FontFamily>` resources. It should look like in the following code sample. 
```xml
<Applicaton ...>
	<Application.Resources>
		<FontFamily x:Key="NotoSansRegular">./Fonts/NotoSans-Regular.ttf#Noto Sans</FontFamily>
		<FontFamily x:Key="NotoSansItalic">./Fonts/NotoSans-Italic.ttf#Noto Sans</FontFamily>
		<FontFamily x:Key="NotoSansBold">./Fonts/NotoSans-Bold.ttf#Noto Sans</FontFamily>
		<FontFamily x:Key="NotoSansBoldItalic">./Fonts/NotoSans-BoldItalic.ttf#Noto Sans</FontFamily>
		<FontFamily x:Key="NotoSansSymbols">./Fonts/NotoSans-Regular.ttf#Noto Sans Symbols</FontFamily>
	</Application.Resources>
</Application>
```

- Apply your fonts like this: 
```xml
<TextBlock Text="foobar" FontFamily="{StaticResource NotoSansRegular}" 
           FontSize="10.0" FontStyle="Normal" FontWeight="Regular" />
```

### WinForms: Load Packaged Font

Create a Font Manager
```csharp
using System;
using System.Drawing;
using System.Drawing.Text;
using System.IO;
using System.Reflection;
using System.Runtime.InteropServices;
```

```csharp
public sealed class FontManager : IDisposable
{
	private PrivateFontCollection privateFonts = new PrivateFontCollection();

	public FontFamily ArimoRegular { get; private set; }
	public FontFamily ArimoBold { get; private set; }
	// Add other FontFamily properties as needed

	public FontManager()
	{
		// Load fonts from embedded resources
		ArimoRegular = LoadFont("YourNamespace.ArimoNerdFont-Regular.ttf");
		ArimoBold = LoadFont("YourNamespace.ArimoNerdFont-Bold.ttf");
		// Load other fonts similarly
	}

	private FontFamily LoadFont(string resourceName)
	{
		// Read font data from embedded resource
		byte[] fontData = GetFontData(resourceName);

		// Allocate memory and copy font data
		IntPtr fontPtr = Marshal.AllocCoTaskMem(fontData.Length);
		Marshal.Copy(fontData, 0, fontPtr, fontData.Length);

		// Add font to PrivateFontCollection
		privateFonts.AddMemoryFont(fontPtr, fontData.Length);

		// Free the unmanaged memory
		Marshal.FreeCoTaskMem(fontPtr);

		// Return the loaded FontFamily
		return privateFonts.Families[privateFonts.Families.Length - 1];
	}

	private byte[] GetFontData(string resourceName)
	{
		Assembly assembly = Assembly.GetExecutingAssembly();
		using Stream stream = assembly.GetManifestResourceStream(resourceName);

		if (stream == null)
		{
			throw new Exception($"Resource '{resourceName}' not found.");
		}

		byte[] data = new byte[stream.Length];
		stream.Read(data, 0, data.Length);
		return data;
	}

	public void Dispose() => privateFonts.Dispose();
}
```

Use the fonts like this:
```csharp
using (var fontManager = new FontManager())
{
    Font customFont = new Font(fontManager.ArimoRegular, 12f); label1.Font = customFont;
    // ...
}
```

### WinForms: Load Font from Output Directory

```csharp
private FontFamily LoadFontFromFile(string fontFileName)
{
	string fontPath = Path.Combine(Application.StartupPath, fontFileName);
	privateFonts.AddFontFile(fontPath);
	FontFamily fontFamily = privateFonts.Families[privateFonts.Families.Length - 1];
	return fontFamily;
}
```

### References 

* [MSDN: Packaging Fonts with Applications](https://msdn.microsoft.com/en-us/library/ms753303(v=vs.110).aspx)
