CodeDOM is part of the .NET Framework Class Library (FCL) and is located in
- `System.CodeDom`
- `System.CodeDom.Compiler`

CodeDOM uses code graphs to express code as data. The code graph can be created by parsing souce code, or adding objects imperatively.

The code graph can be compiled into IL code for execution.

## LINQ Expression Tree vs. CodeDOM code graph

The CodeDOM code graph is a representation of .NET code and differs from the LINQ [[Expression Trees]].

Typically an expression tree is parsed to create either SQL code or an CodeDOM code tree, while che code tree is used to compile into IL code.

## CodeDOM objects

| Class                 | Description                                              | Example                                  | Example Representation          |
| --------------------- | -------------------------------------------------------- | ---------------------------------------- | ------------------------------- |
| `CodeObject`          | Base class of all other code objects                     | `new CodeObject()`                       | N/A (base class)                |
| `CodeNamespaceImport` | Represents an import directive of a namespace            | `new CodeNamespaceImport("System")`      | `using System;`                 |
| `CodeNamespace`       | Represents a namespace declaration                       | `new CodeNamespace("MyNamespace")`       | `namespace MyNamespace { ... }` |
| `CodeCompileUnit`     | Represents a container for a CodeDOM program graph       | `new CodeCompileUnit()`                  | N/A (container for code graph)  |
| `CodeStatement`       | Represents the base class for all code statements        | `new CodeStatement()`                    | N/A (base class)                |
| `CodeTypeMember`      | Represents a member of a type (e.g., method, property)   | `new CodeTypeMember()`                   | N/A (base class)                |
| `CodeTypeParameter`   | Represents a type parameter for a generic type or method | `new CodeTypeParameter("T")`             | `T`                             |
| `CodeTypeReference`   | Represents a reference to a type                         | `new CodeTypeReference("System.String")` | `System.String`                 |
| `CodeExpression`      | Represents the base class for all code expressions       | `new CodeExpression()`                   | N/A (base class)                |
| `CodeDirective`       | Represents a compiler directive                          | `new CodeDirective()`                    | `#directive`                    |
| `CodeComment`         | Represents a comment in the code                         | `new CodeComment("This is a comment")`   | `// This is a comment`          |

### `CodeExpression`s

| Class                                | Description                                                                           | Example                                                         | Example Representation |
| ------------------------------------ | ------------------------------------------------------------------------------------- | --------------------------------------------------------------- | ---------------------- |
| `CodeArgumentReferenceExpression`    | Represents a reference to a parameter passed to a method.                             | `new CodeArgumentReferenceExpression("arg1")`                   | `arg1`                 |
| `CodeBinaryOperatorExpression`       | Represents an expression that consists of a binary operation between two expressions. | `new CodeBinaryOperatorExpression(lhs, op, rhs)`                | `a + b`                |
| `CodeCastExpression`                 | Represents an expression cast to a different data type.                               | `new CodeCastExpression(typeof(int), expr)`                     | `(int)myVariable`      |
| `CodeFieldReferenceExpression`       | Represents a reference to a field.                                                    | `new CodeFieldReferenceExpression(target, "field")`             | `myObject.field`       |
| `CodeMethodInvokeExpression`         | Represents an expression that invokes a method.                                       | `new CodeMethodInvokeExpression(target, "Method")`              | `myObject.Method()`    |
| `CodeParameterDeclarationExpression` | Represents a parameter declaration for a method, property, or constructor.            | `new CodeParameterDeclarationExpression(typeof(int), "param1")` | `int param1`           |
| `CodePrimitiveExpression`            | Represents a primitive data type value.                                               | `new CodePrimitiveExpression(42)`                               | `42`                   |
| `CodePropertyReferenceExpression`    | Represents a reference to a property.                                                 | `new CodePropertyReferenceExpression(target, "Property")`       | `myObject.Property`    |
| `CodeThisReferenceExpression`        | Represents a reference to the current instance.                                       | `new CodeThisReferenceExpression()`                             | `this`                 |
| `CodeVariableReferenceExpression`    | Represents a reference to a local variable.                                           | `new CodeVariableReferenceExpression("variable")`               | `variable`             |

### `CodeStatement`s

| Class                              | Description                                                         | Example                                                                     | Example Representation                                    |
| ---------------------------------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------- | --------------------------------------------------------- |
| `CodeAssignStatement`              | Represents a statement that assigns a value to a variable.          | `new CodeAssignStatement(varRef, expr)`                                     | `variable = value;`                                       |
| `CodeConditionStatement`           | Represents a conditional branch statement, such as an if statement. | `new CodeConditionStatement(condition, trueStatements, falseStatements)`    | `if (condition) { ... } else { ... }`                     |
| `CodeIterationStatement`           | Represents a loop statement, such as a for or while loop.           | `new CodeIterationStatement(initStmt, testExpr, incrementStmt, statements)` | `for (int i = 0; i < n; i++) { ... }`                     |
| `CodeMethodReturnStatement`        | Represents a return statement from a method.                        | `new CodeMethodReturnStatement(expr)`                                       | `return value;`                                           |
| `CodeThrowExceptionStatement`      | Represents a statement that throws an exception.                    | `new CodeThrowExceptionStatement(expr)`                                     | `throw new Exception();`                                  |
| `CodeTryCatchFinallyStatement`     | Represents a try-catch-finally statement.                           | `new CodeTryCatchFinallyStatement(tryStmts, catchClauses, finallyStmts)`    | `try { ... } catch (Exception e) { ... } finally { ... }` |
| `CodeVariableDeclarationStatement` | Represents a statement that declares a new variable.                | `new CodeVariableDeclarationStatement(type, name, initExpr)`                | `int x = 10;`                                             |

### `CodeTypeMember`s

| Class                  | Description                                            | Example                                                    | Example Representation                |
| ---------------------- | ------------------------------------------------------ | ---------------------------------------------------------- | ------------------------------------- |
| `CodeMemberField`      | Represents a member field in a type.                   | `new CodeMemberField(typeof(int), "myField")`              | `public int myField;`                 |
| `CodeMemberProperty`   | Represents a property of a type.                       | `new CodeMemberProperty("MyProperty", PropertyAttributes)` | `public int MyProperty { get; set; }` |
| `CodeMemberMethod`     | Represents a method of a type.                         | `new CodeMemberMethod("MyMethod", methodAttributes)`       | `public void MyMethod() { ... }`      |
| `CodeMemberEvent`      | Represents an event in a type.                         | `new CodeMemberEvent("MyEvent", typeof(EventHandler))`     | `public event EventHandler MyEvent;`  |
| `CodeConstructor`      | Represents a constructor of a type.                    | `new CodeConstructor()`                                    | `public MyClass() { ... }`            |
| `CodeTypeConstructor`  | Represents a static constructor of a type.             | `new CodeTypeConstructor()`                                | `static MyClass() { ... }`            |
| `CodeEntryPointMethod` | Represents the entry point method (e.g., Main method). | `new CodeEntryPointMethod()`                               | `public static void Main() { ... }`   |

## Custom CodeDOM provider

Add the compiler in the `app.config` file:

`app.config`
```xml
<configuration>
  <system.codedom>
	<compilers>
	  <compiler
	    language="k#;ks;ksharp"
	    extension=".ks;ks"
	    type="KSharp.KSharpCodeProvider;KSharpCodeProvider"
	    warningLevel="3"
	    compilerOptions="..."
	</compilers>
  </system.codedom>
</configuration>
```

List the loaded code providers to check if the provider is available:

```csharp
using System.CodeDom.Compiler;

foreach (CompilerInfo ci in CodeDomProvider.GetAllCompilerInfo())
{
    Console.WriteLine(string.Join("; ", ci.GetLanguages()));
}
```

Load an CodeDOM provider:

```csharp
// default CodeDOM provider
var cSharpProvider = CodeDomProvider.CreateProvider("c#");
var cSharpProvider = new Microsoft.CSharp.CSharpCodeProvider();

// custom CodeDOM provider
var kSharpProvider = CodeDomProvider.CreateProvider("k#");
var kSharpProvider = new KSharp.KSharpCodeProvider();
```

Compiler parameters:

```csharp
string sourceCode = /*...*/;

var providerOptions = new Dictionary<string, string>
{
	{ "CompilerVersion", "v11.0" }
};

var provider = new Microsoft.CSharp.CSharpCodeProvider(providerOptions);

var compilerParameters = new string[] { /*...*/ };
CompilerResults results = provider.CompileAssemblyFromSource(compilerParameters), sourceCode);
```

## Compiler Features

The compiler declares a list of supported *Compiler Features* using the [GeneratorSupport](https://learn.microsoft.com/en-us/dotnet/api/system.codedom.compiler.generatorsupport) flags:
```csharp
var languageOptions = provider.LanguageOptions;
```

Those flags indicate if a given CodeDOM element is supported.

Compiling a CodeDOM tree with elements that are not supported by the used compiler will result in an exception.

## References

- [Boo.Lang.CodeDom](https://github.com/bamboo/boo/tree/master/src/Boo.Lang.CodeDom)
