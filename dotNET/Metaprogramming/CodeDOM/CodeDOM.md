CodeDOM is part of the .NET Framework Class Library (FCL) and is located in
- `System.CodeDom`
- `System.CodeDom.Compiler`

CodeDOM uses code graphs to express code as data. The code graph can be created by parsing souce code, or adding objects imperatively.

The code graph can be compiled into IL code for execution.

See [[CodeDOM Code Graph Objects]] for a list of Code Graph object.

## Important Note

The `CodeDOM` API is obsolete and only supports old versions of C#/VB. When current version is required, consider using the [[Roslyn]] API instead.

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

## Example

```csharp
partial class HelloWorldCodeDOM
{
    static CodeNamespace BuildProgram()
    {
        // create a namespace
        var ns = new CodeNamespace("MyNamespace");

        // usings
        var systemImport = new CodeNamespaceImport("System");
        ns.Imports.Add(systemImport);

        // class
        var programClass = new CodeTypeDeclaration("Program");
        ns.Types.Add(programClass);

        // method
        var methodMain = new CodeMemberMethod
        {
            Attributes = MemberAttributes.Static,
            Name = "Main"
        };
        programClass.Members.Add(methodMain);

        // method implementation
        var statement = new CodeMethodInvokeExpression(new CodeSnippetExpression("Console"), "WriteLine", new CodePrimitiveExpression("Hello, world!"));
        methodMain.Statements.Add(statement);

        return ns;
    }
}
```

## See also

- [[CodeDOM Providers]]
- [[Custom CodeDOM Providers]]
- [[CodeDOM Code Graph Objects]]

## References

- [Boo.Lang.CodeDom](https://github.com/bamboo/boo/tree/master/src/Boo.Lang.CodeDom)
