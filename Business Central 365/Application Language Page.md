# Pages in Application Language

| Page Type         | Description                                              |
| ----------------- | -------------------------------------------------------- |
| List              | list of records                                          |
| Card              | detail view of an record                                 |
| Document          | detail view of an record; with an list of child-elements |
| Journal/Worksheet | list of records with detail-view on bottom               |

## Card
```json
page 50200 "CardName"
{
    PageType = Card;
    SourceTable = "TableName";
    layout
    {
        area(content)
        {
            group("GroupName")
            {
                field("ColumnName", "CloumnLabel") { ApplicationArea = "Basic"; }
                // more fields
            }
        }
    }
}
```
## List
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

## Package files
- Open `AL: Package` to create package that is deployed to Business Central

### Edit `.docx` file
- Open Document in MS Word
- Enable Developer tab in `Options` / `Customize Ribbon` / `[x] Developer`
- Select Developer Tab in Word and select `XML Mapping Pane`

### Edit `.rdlc` file
- Open in `SQL Server Report Designer`

