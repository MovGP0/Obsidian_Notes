In Solution File
```xml
<Project Sdk="Microsoft.NET.Sdk">
	<PropertyGroup>
		<!-- ... -->
		<ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
	</PropertyGroup>
	<ItemGroup>
		<!-- ... -->
	</ItemGroup>
</Project>
```

In `Directory.packages.props`
```xml
<Project>  
	<ItemGroup>  
		<PackageVersion Include="PACKAGENAME" Version="VERSION" />
	</ItemGroup>  
</Project>
```