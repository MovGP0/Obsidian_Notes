# Code Units in Application Language
Used to declare executable procedures

```al
codeunit 10100 CodeUnitName
{
    trigger OnRun() // event to handle
    begin
      // call procedure here
    end;
    var // declare varibales in var block
        myInt: Integer;
}
```

