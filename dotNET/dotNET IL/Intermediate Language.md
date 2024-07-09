# .NET Intermediate Language (IL) Cheat-Sheet

- IL operates on a stack-based model. Values and operations are pushed onto and popped off the stack.
- Operations like `ldloc`, `stloc`, `ldarg`, and `starg` are used to work with local variables and method arguments.
- Methods are invoked using `call` operations.
- try-catch-finally blocks are used for exception handling.
- Inline-Comments are declared using `//`. Multi-line comments are declared using `/*` and `*/`.

## Basic Structure

```IL
// Assembly declaration
.assembly AssemblyName {}

// Class declaration
.class public auto ansi beforefieldinit ClassName extends [mscorlib]System.Object
{
    // Method declaration
    .method public hidebysig static void Main() cil managed
    {
        .entrypoint
        // Method body
        .maxstack 8
        .locals init (int32 V_0)

        // Instructions
        ldc.i4.s 42     // Load constant 42 onto the stack
        stloc.0         // Store value from stack into local variable 0
        ldloc.0         // Load local variable 0 onto the stack
        call void [mscorlib]System.Console::WriteLine(int32) // Call method
        ret             // Return from method
    }
}
```

## References

- [MSIL (CIL) Overview](https://learn.microsoft.com/en-us/dotnet/standard/managed-code#msil)
- [Common Type System (CTS)](https://learn.microsoft.com/en-us/dotnet/standard/base-types/common-type-system)

## Tools

- [IL Assembler (ILASM)](https://learn.microsoft.com/en-us/dotnet/framework/tools/ilasm-exe-il-assembler)
- [IL Disassembler (ILDASM)](https://learn.microsoft.com/en-us/dotnet/framework/tools/ildasm-exe-il-disassembler)
