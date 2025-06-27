Indicates whether a class converts property change events to ListChanged events.
Occurs when the list changes or an item in the list changes.

## Example

```csharp
public class YourCollection : IRaiseItemChangedEvents
{
    private List<object> items = new List<object>();

    // Implement the required members of the IRaiseItemChangedEvents interface

    // PropertyChange event implementation
    public event PropertyChangedEventHandler PropertyChanged;

    // Determines whether the collection raises the PropertyChanged event for individual items
    public bool RaisesItemChangedEvents => true;

    // Additional members of the IRaiseItemChangedEvents interface

    // Raise the PropertyChanged event for an item in the collection
    protected virtual void OnItemPropertyChanged(object item, string propertyName)
    {
        PropertyChanged?.Invoke(item, new PropertyChangedEventArgs(propertyName));
    }

    // Add other methods and properties of your collection class as needed
}
```

## See also

- [IRaiseItemChangedEvents](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.iraiseitemchangedevents)
- [IBindingList.ListChanged Event](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.ibindinglist.listchanged)
