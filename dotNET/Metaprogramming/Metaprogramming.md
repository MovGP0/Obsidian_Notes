List installed .NET compilers

```csharp
using System.CodeDom.Compiler;

foreach (CompilerInfo ci in CodeDomProvider.GetAllCompilerInfo())  
{
	Console.WriteLine(string.Join(", ", ci.GetLanguages()));  
}
```
