Use optimized Build Configurations for Performance Testing in Release mode:
```xml
<PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Debug|AnyCPU'">
	<DebugSymbols>true</DebugSymbols>
	<DebugType>full</DebugType>
	<Optimize>false</Optimize>
	<OutputPath>bin\Debug\</OutputPath>
	<PlatformTarget>AnyCPU</PlatformTarget>
	<Prefer32bit>false</Prefer32bit>
</PropertyGroup>

<PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Release|AnyCPU'">
	<DebugType>pdbonly</DebugType>
	<Optimize>true</Optimize>
	<OutputPath>bin\Release\</OutputPath>
	<PlatformTarget>x64</PlatformTarget>
</PropertyGroup>
```

Explicitly optimize program while compiling
```powershell
csc /optimize Program.cs
msbuild /p:Configuration=Release MyProject.csproj
dotnet build --configuration Release MyProject.csproj
```

Consider using AOT instead of JIT
- NGen (.NET Framework)
- CrossGen (.NET Core)
- Mono AOT
- .NET Native (UWP)
- CoreRT (.NET Core Runtime optimized for AOT compiled code)
- RuntimeHelpers (AOT compiling at runtime)

*Note* AOT reduces cold start time, but increases build times and might result in less optimized code.

