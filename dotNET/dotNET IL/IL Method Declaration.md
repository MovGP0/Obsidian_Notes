
```IL
.method [visibility] [attributes] [return-type] MethodName ([parameters]) cil managed
{
    // Method body
}
```

### Example

```IL
.method public hidebysig static int32 Add(int32 a, int32 b) cil managed
{
    .maxstack 2
    .locals init (int32 V_0)

    ldarg.0        // Load first argument
    ldarg.1        // Load second argument
    add            // Add the values
    stloc.0        // Store the result in local variable 0
    ldloc.0        // Load the result from local variable 0
    ret            // Return the result
}
```