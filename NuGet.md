Get detailed information for NuGet package using https://nuget.info
```
https://nuget.info/packages/Newtonsoft.Json/12.0.0
```

You can also use the [NuGetPackageExplorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer).

## Project Files

Modify `*.csproj` file for NuGet support

```xml
<PropertyGroup Label="NuGet">  
  <Authors>Foo Bar</Authors>
  <Copyright>Copyright 2024</Copyright>
  <Description>Helper classes</Description>
  <IsPackable>true</IsPackable>
  <PackageId>Package.Name</PackageId>
  <PackageProjectUrl>http://foo.com/package.name</PackageProjectUrl>
  <PackageTags>utils</PackageTags>
  <RepositoryType>git</RepositoryType>
  <RepositoryUrl>http://foo.com/package.name.git</RepositoryUrl>
  <SymbolPackageFormat>snupkg</SymbolPackageFormat>
  <GenerateDocumentationFile>true</GenerateDocumentationFile>
  <IncludeSymbols>true</IncludeSymbols>
</PropertyGroup>
```

## Source Link

To enable [SourceLink](https://github.com/dotnet/sourcelink), additional packages are required:

```xml
  <ItemGroup>
    <SourceLinkAzureDevOpsServerGitHost Include="my.devops.com:8080" />
  </ItemGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.SourceLink.AzureDevOpsServer.Git" Version="8.0.0" PrivateAssets="All" />
  </ItemGroup>
```

Instead of `RepositoryUrl` we can use `PublishRepositoryUrl` to create the URL automatically on build:

```xml
<PublishRepositoryUrl>true</PublishRepositoryUrl>
```

## Publish Package

```powershell
dotnet pack --configuration 'Release'
dotnet nuget push 'bin/Release/MyLibrary.1.0.0.nupkg' --api-key 'APIKEY' --source 'https://api.nuget.org/v3/index.json'
```