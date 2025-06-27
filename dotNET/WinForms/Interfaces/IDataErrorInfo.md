Provides the functionality to offer custom error information that a user interface can bind to.

Typically implemended by deriving from the [DataErrorValidationRule](https://learn.microsoft.com/en-us/dotnet/api/system.windows.controls.dataerrorvalidationrule) class used in the data binding.

## Example

```csharp
public class YourClass : IDataErrorInfo
{
    // Your class properties
    
    public string? Error
    {
        // Implement the Error property if you have any validation rules that apply to the entire object.
        get
        {
            // Return any validation error message that applies to the entire object.
            // If there are no errors, return null or an empty string.
            return null;
        }
    }
    
    public string? this[string columnName]
    {
        get
        {
            // Implement the indexer property to provide validation error messages for individual properties.
            // Check the columnName parameter to determine which property to validate.
            // Return any validation error message for the specified property.
            
            string error = string.Empty;
            
            // Perform validation based on the columnName
            switch (columnName)
            {
                case "PropertyName1":
                    // Validation logic for PropertyName1
                    // If there is an error, set the error variable
                    break;
                case "PropertyName2":
                    // Validation logic for PropertyName2
                    // If there is an error, set the error variable
                    break;
                // Add cases for other properties
            }
            
            return error;
        }
    }
}
```

## See also

- [IDataErrorInfo](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.idataerrorinfo)