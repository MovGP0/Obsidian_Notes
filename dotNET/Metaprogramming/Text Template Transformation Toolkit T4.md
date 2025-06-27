T4 templates are processed using a custom tool called the `TextTemplatingFileGenerator`. 

The T4 generator must be declared as the generator to be used in the `.csproj` file and gets executed by `MSBuild`:

```xml
<ItemGroup>
  <None Update="SomeTemplate.tt">
    <Generator>TextTemplatingFileGenerator</Generator>
    <LastGenOutput>SomeTemplate.cs</LastGenOutput>
  </None>
</ItemGroup>
```

Note that other generators also exist:

| Generator                    | Usage                                 |
| ---------------------------- | ------------------------------------- |
| `DataServiceClientGenerator` | Creates OData access clients          |
| `EntityModelCodeGenerator`   | Creates classes from EF models        |
| `MSLinqToSQLGenerator`       | Creates classes from LinqToSql models |
| `ResXFileCodeGenerator`      | Creates classes from .resx files      |
| `WCFProxyGenerator`          | Creates web service proxy classes     |

## Header

```t4
<#@ template 
    debug="false"
    hostspecific="true"
    language="C#"
    compilerOptions="warnaserror+"
    culture="en-US" #>
```

Write to file with a spefic extension

```t4
<#@ output extension=".cs" encoding="utf-8" #>
```

Prevent output
```t4
<#@ output extension="/" #>
```

Load external assembly

```t4
<#@ assembly name="System.Core" #>
```

Import namespaces

```t4
<#@ import namespace="System.Collections.Generic" #>
<#@ import namespace="System.IO" #>
<#@ import namespace="System.Linq" #>
<#@ import namespace="System.Text" #>
```

## Control block / Statememt block

A control block is between `<#` and `#>`.

```csharp
<#
var fileContent = GenerateFileContent();
var templateDirectory = Path.GetDirectoryName(Host.TemplateFile);
var filePath = $"{fileName}.generated.cs";
File.WriteAllText(Path.Combine(templateDirectory!, filePath), fileContent);
#>
```

## Class Feature Block

A class feature block is between `<#+` and `#>`. Its used to declare custom classes or methods.

Declare a custom method
```csharp
<#+
private static string GenerateFileContent()
{
    // ...
}
#>
```

Declare a custom type
```csharp
<#+
public sealed class MyClass
{
    // ...
}
#>
```

## Expression Code Block

An expression code block is between `<#=` and `#>`. It outputs the value of an object using the `.ToString()` method.

```csharp
<#= myObject.SomeValue #>
```

## Example

```t4
<#@ template language="C#" #>
<#@ output extension=".cs" #>
<#@ assembly name="System.Core" #>
<#@ import namespace="System.Linq" #>
<#
  Type[] types_to_generate = new[]
  {
    typeof(object), typeof(bool),    typeof(byte),
    typeof(char),   typeof(decimal), typeof(double),
    typeof(float),  typeof(int),     typeof(long),
    typeof(sbyte),  typeof(short),   typeof(string),
    typeof(uint),   typeof(ulong),   typeof(ushort)
  };
#>
using System;

public static class Greater
{
<#
  foreach (var type in types_to_generate)
  {
#>
  public static <#= type.Name #> of(<#= type.Name #> left, <#= type.Name #> right)
  {
<#
    Type icomparable =
      (from intf in type.GetInterfaces() where typeof(IComparable<>)
          .MakeGenericType(type)
          .IsAssignableFrom(intf)
        || typeof(IComparable).IsAssignableFrom(intf)
      select intf).FirstOrDefault();

    if (icomparable != null)
    {
#>
    return left.CompareTo(right) < 0 ? right : left;
<#
    }
    else
    {
#>
    throw new ApplicationException(
      "Type <#= type.Name #> must implement one of the " +
      "IComparable or IComparable<<#= type.Name #>> interfaces.");
<#
    }
#>
  }
<#
}
#>
}
```
