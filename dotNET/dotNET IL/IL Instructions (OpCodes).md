
## Common IL Instructions

| Instruction | Description                                | Example                                |
| ----------- | ------------------------------------------ | -------------------------------------- |
| `add`       | Add two values                             | `add`                                  |
| `arg`       | Reference an argument                      | `arg.0`                                |
| `br`        | Break to a specified point in a method     | `br.s Label1`                          |
| `call`      | Call a method                              | `call void [mscorlib]System.Console::WriteLine(string)` |
| `div`       | Divide two values                          | `div`                                  |
| `ld`        | Load a value                               | `ldloc.0`                              |
| `ldarg`     | Load argument onto the stack               | `ldarg.0`                              |
| `ldc.i4`    | Load a 4-byte integer onto the stack       | `ldc.i4 42`                            |
| `ldc`       | Load a constant value                      | `ldc.r4 3.14`                          |
| `ldloc`     | Load local variable onto the stack         | `ldloc.1`                              |
| `ldstr`     | Load a string onto the stack               | `ldstr "Hello, World!"`                |
| `mul`       | Multiply two values                        | `mul`                                  |
| `nop`       | No operation                               | `nop`                                  |
| `ret`       | Return from method                         | `ret`                                  |
| `st`        | Store a value                              | `stloc.0`                              |
| `starg`     | Store value from stack into argument       | `starg.s 1`                            |
| `stloc`     | Store value from stack into local variable | `stloc.1`                              |
| `sub`       | Subtract two values                        | `sub`                                  |
| `ovf`       | Overflow detection                         | `add.ovf`                              |
| `conf`      | Type conversion                            | `conv.i4`                              |
| `elem`      | Element of an array                        | `ldelem.i4`                            |
| `fld`       | Use a field                                | `ldfld int32 MyClass::myField`         |

## References

- [MSIL Instruction Set](https://learn.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes)
