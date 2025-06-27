Implement the CodeDOM provider by deriving from the [System.CodeDom.Compiler.CodeDomProvider](https://learn.microsoft.com/en-us/dotnet/api/system.codedom.compiler.codedomprovider) class.

## Register Custom CodeDOM provider

Add the compiler in the `app.config` file:

`app.config`
```xml
<configuration>
  <system.codedom>
	<compilers>
	  <compiler
	    language="k#;ks;ksharp"
	    extension=".ks;ks"
	    type="KSharp.KSharpCodeProvider;KSharpCodeProvider, Version=1.0.0.0, Culture=neutral, PublicKeyToken=..."
	    warningLevel="3"
	    compilerOptions="..."
	</compilers>
  </system.codedom>
</configuration>
```

Load the custom CodeDOM provider:

```csharp
CodeDomProvider kSharpProvider = CodeDomProvider.CreateProvider("k#");
CodeDomProvider kSharpProvider = new KSharp.KSharpCodeProvider();
```

## Register custom CodeDOM provider globally

If the CodeDOM provider should be installed globally

1. Create a strong name key file
```bash
sn -k KSharpCodeProvider.snk
```

2. Add the key to the project file
```csharp
[assembly: AssemblyKeyFile("KSharpCodeProvider.snk")]
```

2. add the assembly to the Global Assembly Cache (GAC)
```bash
gacutil -i ~\KSharpCodeProvider.dll
```

3. Register the provider in the `machine.config` file:
```text
C:\Windows\Microsoft.NET\Framework\<version>\config\machine.config
```
```xml
<configuration>
  <system.codedom>
	<compilers>
	  <compiler
	    language="k#;ks;ksharp"
	    extension=".ks;ks"
	    type="KSharp.KSharpCodeProvider;KSharpCodeProvider, Version=1.0.0.0, Culture=neutral, PublicKeyToken=..."
	    warningLevel="3"
	    compilerOptions="..."
	</compilers>
  </system.codedom>
</configuration>
```
