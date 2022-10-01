# Report in Application Language
```json
report 50201 "ListName"
{
    UsageCategory = ReportsAndAnalysis;
    ApplicationArea = Basic;
    WordLayout = "ListTemplate.docx";
    RDLCLayout = "ListTemplate.rdlc";
    dataset
    {
        dataitem(DataItemName; "TableName")
        {
            column("ColumnName"; "FieldName")
        }
    }
}
```