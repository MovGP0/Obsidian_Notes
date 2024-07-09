An ExpressionTree represents code as data.

Note that code that is represented by an expression tree cannot be invoked directly, but needs to be compiled. The resulting delegate (pointer to the code) that is returned form the compile operation can be invoked.

## Example

Create expression tree
```csharp
Expression<Func<int, int, bool>> greaterThanExpr = (Left, Right) => Left > Right;
```

Compile expression tree to code
```csharp
Func<int, int, bool> greaterThan = greaterThanExpr.Compile();
```

Invoke the compiled code (via delegate)
```csharp
bool result = greaterThan(7, 11);
```

## Expression types

| Expression Type          | Description                                             |
|--------------------------|---------------------------------------------------------|
| `BinaryExpression`      | Add, Multiply, Modulo, GreaterThan, LessThan, and so on |
| `BlockExpression`       | Acts as a container for a sequence of other Expressions |
| `ConditionalExpression` | IfThen, IfThenElse, and so on                           |
| `GotoExpression`        | For branching and returning to LabelExpressions         |
| `IndexExpression`       | For array and property access                           |
| `MethodCallExpression`  | For invoking methods                                    |
| `NewExpression`         | For calling constructors                                |
| `SwitchExpression`      | For testing object equivalence against a set of values  |
| `TryExpression`         | For implementing exception handling                     |
| `UnaryExpression`       | Convert, Not, Negate, Increment, Decrement, and so on   |
