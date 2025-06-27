## Numeric data types

- int: Integer (whole number) data from -2^31 to 2^31-1
- bigint: Integer data from -2^63 to 2^63-1
- smallint: Integer data from -2^15 to 2^15-1
- tinyint: Integer data from 0 to 255
- decimal/numeric: Fixed precision and scale numbers
- float: Approximate numeric values with a floating decimal point
- real: Single-precision floating point number

## Date and time data types

- datetime: Date and time values from January 1, 1753, to December 31, 9999, with an accuracy of 3.33 milliseconds
- smalldatetime: Date and time values from January 1, 1900, to June 6, 2079, with an accuracy of one minute
- date: Date values from January 1, 0001, to December 31, 9999
- time: Time values with an accuracy of 100 nanoseconds
- datetime2: Date and time values with an accuracy of 100 nanoseconds
- datetimeoffset: Date and time values with a time zone offset

## Character string data types

- char(n): Fixed-length non-Unicode character strings with a maximum length of 8,000 characters
- varchar(n): Variable-length non-Unicode character strings with a maximum length of 8,000 characters
- varchar(max): Variable-length non-Unicode character strings with a maximum length of 2^31-1 characters
- nchar(n): Fixed-length Unicode character strings with a maximum length of 4,000 characters
- nvarchar(n): Variable-length Unicode character strings with a maximum length of 4,000 characters
- nvarchar(max): Variable-length Unicode character strings with a maximum length of 2^30-1 characters
- text: Variable-length non-Unicode character strings with a maximum length of 2^31-1 characters (deprecated)
- ntext: Variable-length Unicode character strings with a maximum length of 2^30-1 characters (deprecated)

## Binary data types:
    
- binary(n): Fixed-length binary data with a maximum length of 8,000 bytes
- varbinary(n): Variable-length binary data with a maximum length of 8,000 bytes
- varbinary(max): Variable-length binary data with a maximum length of 2^31-1 bytes
- image: Variable-length binary data with a maximum length of 2^31-1 bytes (deprecated)

## Other data types

- bit: Integer data with either a 1 or 0 value
- uniqueidentifier: A globally unique identifier (GUID)
- cursor: A reference to a cursor
- xml: Stores XML data
- sql_variant: Stores values of various data types, except text, ntext, image, timestamp, and any user-defined data types
- hierarchyid: Represents a position in a hierarchical tree structure
- spatial data types: geometry and geography for storing spatial data
- table: Stores a result set for later processing
