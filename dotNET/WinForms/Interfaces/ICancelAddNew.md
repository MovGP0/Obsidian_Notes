
## Example

```csharp
public class YourCollection : IBindingList, ICancelAddNew
{
    private List<object> items = new List<object>();
    private List<object> addedItems = new List<object>();

    // Implement the required members of the IBindingList interface
    
    // PropertyChange event implementation
    public event ListChangedEventHandler ListChanged;

    // Add new object to the collection
    public object AddNew()
    {
        object newItem = // Create a new object to add to the collection
        items.Add(newItem);
        addedItems.Add(newItem);
        OnListChanged(ListChangedType.ItemAdded, items.Count - 1);
        return newItem;
    }

    // Commit changes made to the new object
    public void EndNew(int itemIndex)
    {
        // Perform any necessary validation or finalization for the new item
        OnListChanged(ListChangedType.ItemChanged, itemIndex);
        addedItems.Remove(items[itemIndex]);
    }

    // Cancel the addition of the new object
    public void CancelNew(int itemIndex)
    {
        if (itemIndex >= 0 && itemIndex < items.Count)
        {
            items.RemoveAt(itemIndex);
            addedItems.RemoveAt(addedItems.Count - 1);
            OnListChanged(ListChangedType.ItemDeleted, itemIndex);
        }
    }

    // Implement the ICancelAddNew interface

    // Cancel the addition of all new objects
    public void CancelNew()
    {
        foreach (object newItem in addedItems)
        {
            items.Remove(newItem);
        }
        addedItems.Clear();
        OnListChanged(ListChangedType.Reset, -1);
    }

    // Additional members of the IBindingList interface and custom methods and properties can be implemented as needed

    // Helper method to raise the ListChanged event
    protected virtual void OnListChanged(ListChangedType type, int index)
    {
        ListChanged?.Invoke(this, new ListChangedEventArgs(type, index));
    }
}
```
