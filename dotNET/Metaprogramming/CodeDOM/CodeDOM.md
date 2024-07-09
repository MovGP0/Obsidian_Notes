CodeDOM is part of the .NET Framework Class Library (FCL) and is located in
- `System.CodeDom`
- `System.CodeDom.Compiler`

CodeDOM uses code graphs to express code as data. The code graph can be created by parsing souce code, or adding objects imperatively.

The code graph can be compiled into IL code for execution.

See [[CodeDOM Code Graph Objects]] for a list of Code Graph object.

## LINQ Expression Tree vs. CodeDOM code graph

The CodeDOM code graph is a representation of .NET code and differs from the LINQ [[Expression Trees]].

Typically an expression tree is parsed to create either SQL code or an CodeDOM code tree, while che code tree is used to compile into IL code.

## Create source code

```csharp
CodeNamespace codeGraph = /* ... */;

var options = new CodeGeneratorOptions
{
    BracingStyle = "C",
    IndentString = "    ", // 4 spaces indent
    BlankLinesBetweenMembers = false
};

CodeDomProvider provider = CodeDomProvider.CreateProvider("c#");

var builder = new StringBuilder();
using (var writer = new StringWriter(builder))
{
    provider.GenerateCodeFromNamespace(codeGraph, writer, options);
}

var sourceCode = builder.ToString();
```

## See also

- [[CodeDOM Providers]]
- [[Custom CodeDOM Providers]]
- [[CodeDOM Code Graph Objects]]
- [Boo.Lang.CodeDom](https://github.com/bamboo/boo/tree/master/src/Boo.Lang.CodeDom)
