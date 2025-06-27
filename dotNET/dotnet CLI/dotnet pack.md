`dotnet pack` creates NuGet packages. 

### Create `.snupkg` debug files

`.snupkg` contain debugging symbols like `.pdb` (Program database) files.

```xml
<PropertyGroup>
	<DebugType>portable</DebugType>
	<IncludeSymbols>true</IncludeSymbols>
	<SymbolPackageFormat>snupkg</SymbolPackageFormat>
</PropertyGroup>
```

```powershell
dotnet pack --configuration Release
```
