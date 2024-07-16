Boo is a .NET scripting language that uses a custom AST and compiler.

```csharp
using Boo.Lang.Compiler.Ast

var xParameter = new ParameterDeclaration(name: 'x', type: SimpleTypeReference(name: 'int'))
var yParameter = ParameterDeclaration(name: 'y', type: SimpleTypeReference(name: 'int'));;

var parameters = new ParameterDeclarationCollection();
parameters.Add(xParameter);
parameters.Add(yParameter);

var apiAdd = new Method(
  name: 'LiteralAdd',
  parameters: parameters,
  body: new Block(new ReturnStatement(new BinaryExpression(left: Expression.Lift(xParameter), operator: BinaryOperatorType.Addition)));
```

```csharp
using static Boo.Lang.Compiler.MetaProgramming;

var literalAdd = @"
class QA:
    static def QuotedAdd(x as int, y as int):
        return x + y;
";

dynamic compiledLiteralAdd = Compile(literalAdd);
var result = compiledLiteralAdd.QuotedAdd(5, 6);
```

## Resources

- [Boo Language](https://boo-language.github.io/)