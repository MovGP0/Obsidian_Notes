## Web Projects

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">  
	<PropertyGroup>
		<TargetFramework>net7.0-windows</TargetFramework>
		<UserSecretsId>aspnet-CareCenterServices-b40572a7-7f02-4c5b-89f4-eb72355662a0</UserSecretsId>
		<StartupObject>CareCenterServices.Program</StartupObject>
		<ApplicationIcon>wwwroot\favicon.ico</ApplicationIcon>
		<MvcRazorCompileOnPublish>true</MvcRazorCompileOnPublish>
		<MvcRazorPrecompile>true</MvcRazorPrecompile>
		<PreserveCompilationContext>true</PreserveCompilationContext>
		<AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
		<AspNetCoreModuleName>AspNetCoreModuleV2</AspNetCoreModuleName>
		<Nullable>enable</Nullable>
		<ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
		<ImplicitUsings>true</ImplicitUsings>
		<LangVersion>10.0</LangVersion>
		<NoWarn>1701;1702;NU1507</NoWarn>  
	</PropertyGroup>
</Project>
```

### WPF Projects

```xml
<Project Sdk="Microsoft.NET.Sdk">

	<PropertyGroup>
		<OutputType>WinExe</OutputType>
		<TargetFramework>net6.0-windows8.0</TargetFramework>
		<RuntimeIdentifier>win-x64</RuntimeIdentifier>
		<UseWPF>true</UseWPF>
		<Nullable>Enable</Nullable>
		<ApplicationIcon>kiosk.ico</ApplicationIcon>
		<StartupObject>MyApp.App</StartupObject>
		<ImplicitUsings>enable</ImplicitUsings>
		<ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
		<ApplicationManifest>Properties\app.manifest</ApplicationManifest>
		<SignManifests>false</SignManifests>
		<Company>MyCompany</Company>
		<Product>MyApp</Product>
		<Authors>MyName</Authors>
		<Version>1.0.0</Version>
		<PackageId>MyCompany.MyApp</PackageId>
		<AssemblyVersion>1.0.0.0</AssemblyVersion>
		<FileVersion>1.0.0.0</FileVersion>
		<NeutralLanguage>de-AT</NeutralLanguage>
		<SignAssembly>false</SignAssembly>
	</PropertyGroup>
```

### Unit Test Projects

```xml
<Project Sdk="Microsoft.NET.Sdk">
	<PropertyGroup>
		<TargetFramework>net6.0-windows8.0</TargetFramework>
		<IsPackable>false</IsPackable>
		<Nullable>enable</Nullable>
		<ImplicitUsings>enable</ImplicitUsings>
		<ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
	</PropertyGroup>

	<ItemGroup>
		<DotNetCliToolReference Include="JetBrains.dotCover.CommandLineTools" Version="2023.1.0" />
	</ItemGroup>

	<ItemGroup>
		<PackageReference Include="Microsoft.NET.Test.Sdk" />
		<PackageReference Include="Microsoft.Extensions.Configuration" />
		<PackageReference Include="NodaTime" />
		<PackageReference Include="NSubstitute" />
		<PackageReference Include="NUnit" />
		<PackageReference Include="NUnit3TestAdapter">
			<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
			<PrivateAssets>all</PrivateAssets>
		</PackageReference>
		<PackageReference Include="Roslynator.Analyzers">
			<PrivateAssets>all</PrivateAssets>
			<IncludeAssets>runtime; build; native; contentfiles; analyzers</IncludeAssets>
		</PackageReference>
	</ItemGroup>
</Project>
```