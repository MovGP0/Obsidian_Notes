see also: [Data Types and Methods in AL](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/methods-auto/library)

| Type           | Description                                                                      |
| -------------- | -------------------------------------------------------------------------------- |
| `Code[len]`    | String (uppercase and truncated; max: 250 char)                                  |
| `Text[maxlen]` | String (max: 1024 char; default: 250 char)                                       |
| `Char`         | Char (16 bit)                                                                    |
| `Date`         | DateOnly                                                                         |
| `Duration`     | TimeSpan                                                                         |
| `DateTime`     | DateTime in UTC                                                                  |
| `Byte`         | Integer (8 bit)                                                                  |
| `Integer`      | Integer (32 bit)                                                                 |
| `BigInteger`   | Integer (64 bit)                                                                 |
| `Decimal`      | Decimal                                                                          |
| `Option`       | Enum (32 bit integer; see also `OptionMembers` and `OptionCaptionML` properties) |
| `Boolean`      | Boolean                                                                          |
| `Blob`         | Binary data (see also `Subtype` and `Compressed` properties)                     |

## Field triggers

```json
table 5100 "tableName"
{
    field(1; "FieldName", DataType)
    {
        trigger OnValidate()
        begin
            // code when user entered a value
        end;
        trigger OnLookup()
        begin
            // code when dropdown opens
        end;
    }
}
```

See also: [Events in AL](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-events-in-al)

