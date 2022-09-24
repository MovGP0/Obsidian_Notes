# Tables in Application Language
**Note:** Table names are in Singular (ie. `Product` instead of `Products`)

```json
table 501000 "TableName"
{
	// table properties
	triggers
	{
	    // 
	}
    fields
    {
        field(1; "Field Name"; Type[20])
        {
            // properties
            // triggers
        }
        field(2; "Field Name"; Type[20]) {
	        // properties
	        // triggers
        }
		// additional fields
    }
    keys
    {
        // properties
    }
}
```

## Data types
see also: [Data Types and Methods in AL](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/methods-auto/library)

| Type           | Description                      |
| -------------- | -------------------------------- |
| `Code[len]`    | String (uppercase and truncated) |
| `Text[maxlen]` | String                           |
| `Date`         | DateOnly                         |
| `Duration`     | TimeSpan                         |
| `Decimal`      | Decimal                          |

## Table properties
| Property          | Description |
| ----------------- | ----------- |
| `Caption`           |             |
| `CaptionML`         |             |
| `Description`       |             |
| `DataPerCompany`    |             |
| `Permissions`       |             |
| `LookupPageID`      |             |
| `DrillDownPageID`   |             |
| `DataCaptionFields` |             |
| `PasteIsValid`      |             |
| `LinkedObject`      |             |
| `TableType`         |             |
| `ObsoleteState`     |             |
| `ObsoleteReason`    |             |

## Triggers
| Trigger      |
| ------------ |
| `OnDelete()` |
| `OnInsert()` |
| `OnModify()` |
| `OnRename()` |

When modifying table data using Procedures, one can enable/disable triggers:

| Code                      | Description                         |
| ------------------------- | ----------------------------------- |
| `TableName.INSERT(TRUE)`  | OnInsert() Trigger executes         |
| `TableName.INSERT(FALSE)` | OnInsert() Trigger does not execute |
| `TableName.INSERT`        | Default behaviour                   |

Primary keys will be enforced, even when triggers are not executed.

