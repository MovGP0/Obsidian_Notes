The `CodeObject` class is the base class for the [[CodeDOM]] code graph.

| Class                 | Description                                              | Example                                  |
| --------------------- | -------------------------------------------------------- | ---------------------------------------- |
| `CodeObject`          | Base class of all other code objects                     | `new CodeObject()`                       |

## `CodeObject`s

| Class                 | Description                                              | Example                                  | Example Representation          |
| --------------------- | -------------------------------------------------------- | ---------------------------------------- | ------------------------------- |
| `CodeCompileUnit`     | Represents a container for a CodeDOM program graph       | `new CodeCompileUnit()`                  | N/A (container for code graph)  |
| `CodeNamespaceImport` | Represents an import directive of a namespace            | `new CodeNamespaceImport("System")`      | `using System;`                 |
| `CodeNamespace`       | Represents a namespace declaration                       | `new CodeNamespace("MyNamespace")`       | `namespace MyNamespace { ... }` |
| `CodeTypeParameter`   | Represents a type parameter for a generic type or method | `new CodeTypeParameter("T")`             | `T`                             |
| `CodeTypeReference`   | Represents a reference to a type                         | `new CodeTypeReference("System.String")` | `System.String`                 |
| `CodeDirective`       | Represents a compiler directive                          | `new CodeDirective()`                    | `#directive`                    |
| `CodeComment`         | Represents a comment in the code                         | `new CodeComment("This is a comment")`   | `// This is a comment`          |

Base classes for more specific types:

| Class            | Description                                            |
| ---------------- | ------------------------------------------------------ |
| `CodeStatement`  | Represents the base class for all code statements      |
| `CodeTypeMember` | Represents a member of a type (e.g., method, property) |
| `CodeExpression` | Represents the base class for all code expressions     |

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
