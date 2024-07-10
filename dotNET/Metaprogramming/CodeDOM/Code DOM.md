```csharp
partial class HelloWorldCodeDOM
{
  static CodeNamespace BuildProgram()
  {
    var ns = new CodeNamespace("MetaWorld");
    var systemImport = new CodeNamespaceImport("System");
    ns.Imports.Add(systemImport);
    var programClass = new CodeTypeDeclaration("Program");
    ns.Types.Add(programClass);
    var methodMain = new CodeMemberMethod
    {
      Attributes = MemberAttributes.Static
      , Name = "Main"
    };
    methodMain.Statements.Add(
      new CodeMethodInvokeExpression(
        new CodeSnippetExpression("Console")
        , "WriteLine"
        , new CodePrimitiveExpression("Hello, world!")
      )
    );
    programClass.Members.Add(methodMain);
    return ns;
  }
}
```