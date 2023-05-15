## Example

```csharp
public class YourDataSource : ICurrencyManagerProvider, IBindingList
{
    private BindingContext bindingContext;

    public CurrencyManager CurrencyManager => bindingContext?.GetItemProperties()?.CurrencyManager;

    // Implement the required members of the IBindingList interface
    // Implement the required members of the ICurrencyManagerProvider interface
    // Additional members and properties of your data source class
}
```

```csharp
var bindingSource = new BindingSource();
bindingSource.DataSource = new YourDataSource();
CurrencyManager currencyManager = (CurrencyManager)bindingSource.CurrencyManager;
```

## See also

- [ICurrencyManagerProvider](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.icurrencymanagerprovider)
