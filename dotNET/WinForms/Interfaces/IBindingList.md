Provides the features required to support both complex and simple scenarios when binding to a data source.

Implemented by the `System.Data.DataView` and the derived `System.Windows.Forms.BindingSource` class.

## Example

```csharp
public class YourCollection : IBindingList
{
    private List<object> items = new List<object>();

    // Implement the required members of the IBindingList interface
    
    // PropertyChange event implementation
    public event ListChangedEventHandler ListChanged;

    // Add new object to the collection
    public object AddNew()
    {
        object newItem = // Create a new object to add to the collection
        items.Add(newItem);
        OnListChanged(ListChangedType.ItemAdded, items.Count - 1);
        return newItem;
    }

    // Commit changes made to the new object
    public void EndNew(int itemIndex)
    {
        // Perform any necessary validation or finalization for the new item
        OnListChanged(ListChangedType.ItemChanged, itemIndex);
    }

    // Determine whether the collection supports sorting
    public bool SupportsSorting => false;

    // Sort the collection based on a property descriptor and sort direction
    public void ApplySort(PropertyDescriptor property, ListSortDirection direction)
    {
        throw new NotSupportedException();
    }

    // Remove any sort applied to the collection
    public void RemoveSort()
    {
        throw new NotSupportedException();
    }

    // Determine whether the collection supports filtering
    public bool SupportsFiltering => false;

    // Filter the collection based on a filter string
    public void ApplyFilter(string filter)
    {
        throw new NotSupportedException();
    }

    // Remove any filter applied to the collection
    public void RemoveFilter()
    {
        throw new NotSupportedException();
    }

    // Other members of the IBindingList interface can be implemented as needed

    // Helper method to raise the ListChanged event
    protected virtual void OnListChanged(ListChangedType type, int index)
    {
        ListChanged?.Invoke(this, new ListChangedEventArgs(type, index));
    }

    // Additional custom methods and properties of your collection class
}
```

## See also

- [[ITypedList and BindingList]]
- [IBindingList](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.ibindinglist)
