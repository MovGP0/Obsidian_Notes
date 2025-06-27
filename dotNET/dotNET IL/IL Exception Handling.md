```IL
.try
{
    // Protected code
}
catch [mscorlib]System.Exception
{
    // Exception handler code
}
finally
{
    // Finally block code
}
```

## Example

```IL
.method public hidebysig static void MethodWithException() cil managed
{
    .maxstack 1
    .try
    {
        // Code that might throw an exception
        ldstr "Hello, IL!"
        call void [mscorlib]System.Console::WriteLine(string)
        leave.s EndTry
    }
    catch [mscorlib]System.Exception
    {
        // Exception handling code
        call void [mscorlib]System.Console::WriteLine(string)
        leave.s EndTry
    }
    EndTry: ret
}
```