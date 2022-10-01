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
        field(2; "Field Name"; Type[20])
        {
	        // properties
	        // triggers
        }
        field(3; "Field Name"; Option)
        {
	        OptionMembers = "Foo","Bar","Baz"
        }
        field(4; "Field Name"; Option)
        {
	        OptionMembers = ,"Foo","Bar","Baz" // support null entry
        }
        field(5; "Field Name"; Boolean)
        {
	        InitValue = true;
        }
        
        // dynamic link based on value drop-down
        field(6; "PersonType"; Option)
        {
            OptionMembers = Customer,Employee,Provider
        }
        field(7; "FieldName"; Code[20])
        {
            TableRelation = 
                IF("PersonType" = const("Customer")) "Customer"."Id"
                ELSE IF("PersonType" = const("Employee")) "Employee"."Id"
                ELSE IF("PersonType" = const("Provider")) "Provider"."Id"
        }
        
		// additional fields
    }
    keys
    {
        // primary key
	    key(PK; "FieldName", "FieldName", "...")
	    {
	        Clustered = true;
	        // additional properties
	    }
	    
	    // index with sum
	    key(KeyName; "FieldName", "FieldName", "...")
	    {
		    SumIndexFields = "FieldName","FieldName"; // used for sum row
	    }
	    
        // additional keys
    }
	fieldgroups
	{
	    fieldgroup(DropDown; "Drop-Down Label", TableName, "Field Name")
	    {
		    // properties
	    }
	    fieldgroup(Brick; "Brick Label", TableName, "Field Name")
	    {
		    // properties
	    }
	}
}
```

## Data types
see: [[Application Language DataTypes and Fields]]

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

## Table Types
### Fully modifyable table types
| Table Type                    | Description                                      |
| ----------------------------- | ------------------------------------------------ |
| Master data                   | Primary data of an object                        |
| Journal                       | Table for data entry                             |
| Template                      | Base class for an journal                        |
| Entry table                   | Read-only data ledger                            |
| Subsidiary/Supplemental table | List of codes, descriptions, and validation data |
| Register                      | List of ordered IDs/Timestamps for records       |
| Posted document               | History of changes of an record                  |
| Singleton                     | Table with only one (global) entry               |
| Temporary                     | Table with temporary data                        |

### Content modifyable table types
| Table Type   | Description |
| ------------ | ----------- |
| System table | Management and Administration of Business Central |

### Read-only tables
| Table Type | Description            |
| ---------- | ---------------------- |
| Virtual    | runtime-computed table |
