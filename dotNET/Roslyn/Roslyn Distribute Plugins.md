Configure `.csproj` file
```xml
<PropertyGroup>
  <TargetFramework>netstandard2.0</TargetFramework>
  <PackageId>MyAnalyzer</PackageId>
  <Version>1.0.0</Version>
  <Authors>Your Name</Authors>
  <Description>Your analyzer and code fix provider description</Description>
  <PackageTags>roslyn analyzer codefix</PackageTags>
  <PackageProjectUrl>https://github.com/yourusername/yourrepository</PackageProjectUrl>
  <PackageLicenseExpression>MIT</PackageLicenseExpression>
  <PackageIconUrl>https://your-icon-url.png</PackageIconUrl>
  <RepositoryUrl>https://github.com/yourusername/yourrepository</RepositoryUrl>
  <RepositoryType>git</RepositoryType>
</PropertyGroup>

<ItemGroup>
  <!-- ... -->
</ItemGroup>
```

Pack the NuGet files
```powershell
dotnet pack --configuration Release
```

## Using NuSpec/NuGet

Use .NET Standard 2.0 (or later)

Add references to `.csproj` file
```xml
Microsoft.CodeAnalysis.CSharp.Workspaces
Microsoft.CodeAnalysis.Analyzers
```

Add `.nuspec` file
```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <id>MyAnalyzer</id>
    <version>1.0.0</version>
    <authors>Your Name</authors>
    <owners>Your Name</owners>
    <license type="expression">MIT</license>
    <projectUrl>https://github.com/yourusername/yourrepository</projectUrl>
    <iconUrl>https://your-icon-url.png</iconUrl>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <description>Your analyzer and code fix provider description</description>
    <releaseNotes>Initial release</releaseNotes>
    <tags>roslyn analyzer codefix</tags>
    <dependencies>
      <group targetFramework=".NETStandard2.0">
        <dependency id="Microsoft.CodeAnalysis.CSharp.Workspaces" version="3.11.0" exclude="Build,Analyzers" />
      </group>
    </dependencies>
  </metadata>
  <files>
    <file src="bin\Release\netstandard2.0\*.dll" target="analyzers/dotnet/cs" />
  </files>
</package>
```

Pack the Nuget file
```powershell
nuget pack MyAnalyzer.nuspec
```
