## Declare Variables

```pascal
VariableName : Type "Description"
VariableName : Type[20] "Description" // Type with size limitation
```

## Declare Procedure

```pascal
local procedure ProcedureName(
    ParameterName1 : Type "Description";
    ParameterName2 : Type "Description"
) : ReturnType
var
   // variables are declared in var block
   VariableName1 : Type "Description";
begin
    // implementation comes here
    exit(returnValue);
end;
```
