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
table 50100 "TableName"
{
	LookupPageId = "ListName";
	DrillDownPageId = "ListName";
    fields
    {
       field(1; Code; Code[10]) { } // ID-Spalte
       field(2; Description; Text[50]) {  }
    }
}

page 50200 "ListName"
{
    PageType = List;
    SourceTable = "TableName";
    ApplicationArea = Basic;
    UsageCategory = Administration;
    layout
    {
        area(content)
        {
            group("GroupName")
            {
                field("Code", "Code Label")
                {
	                ApplicationArea = Basic;
	            }
                field("Description", "Description Label")
                { 
	                ApplicationArea = Basic;
	            }
            }
        }
    }
}

table 50300 "AnotherTableName"
{
    fields
    {
	    field(1; "Label"; Code[10]) // referenz auf Tabelle "TableName"
	    {
	        TableRelation = "TableName"
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

