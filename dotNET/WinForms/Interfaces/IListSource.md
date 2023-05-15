Provides functionality to an object to return a list that can be bound to a data source.

Base interface for `DataSet`, `DataTable`, `EntitySet<T>` classes.

## Example

```csharp
public class YourClass : IListSource
{
    public bool ContainsListCollection
    {
        // Implement the ContainsListCollection property to indicate whether the object contains a collection of lists.
        get { return false; } // Set to true if your object contains a collection of lists, otherwise set to false.
    }
    
    public IList? GetList()
    {
        // Implement the GetList method to return the list or collection of your object.
        // This method should return an IList or an IBindingList.

        // If your object is already an IList or IBindingList, you can return it directly.
        // For example, if your object is a List<T>, you can return it like this:
        // return yourList;

        // If your object is not an IList or IBindingList, you need to wrap it in a collection class
        // that implements IList or IBindingList.
        // For example, if your object is a DataTable, you can return it like this:
        // return yourDataTable.DefaultView;

        // If your object does not have a list or collection, you can return null.
        return null;
    }
}
```

## See also

- [IListSource](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.ilistsource)