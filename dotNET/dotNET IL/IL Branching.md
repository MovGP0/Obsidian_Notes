## IL Branching Instructions

| Instruction | Description                     |
| ----------- | ------------------------------- |
| `br`        | Unconditional branch            |
| `brtrue`    | Branch if true (non-zero)       |
| `brfalse`   | Branch if false (zero)          |
| `beq`       | Branch if equal                 |
| `bne.un`    | Branch if not equal (unsigned)  |
| `blt`       | Branch if less than             |
| `bgt`       | Branch if greater than          |
| `ble`       | Branch if less than or equal    |
| `bge`       | Branch if greater than or equal |

## Example

```IL
.method public hidebysig static void ConditionalBranch() cil managed
{
    .maxstack 1
    .locals init (int32 V_0)

    ldc.i4.5        // Load constant 5 onto the stack
    stloc.0         // Store 5 in local variable 0
    ldloc.0         // Load local variable 0 onto the stack
    ldc.i4.5        // Load constant 5 onto the stack
    beq LabelEqual  // Branch if equal

    ldstr "Not equal"
    call void [mscorlib]System.Console::WriteLine(string)
    br End

    LabelEqual:
    ldstr "Equal"
    call void [mscorlib]System.Console::WriteLine(string)

    End:
    ret
}
```