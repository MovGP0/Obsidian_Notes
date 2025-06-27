Subscribes to property changed notifications of items in the collection and notifies the observers

```csharp
using System.Collections;  
using System.Collections.Specialized;  
using System.ComponentModel;

namespace DynamicData.Binding;

public sealed class CustomObservableCollection<T> : ObservableCollectionExtended<T>
    where T : INotifyPropertyChanged
{
    public event EventHandler? ItemsChanged;

    public CustomObservableCollection()
    {
        CollectionChanged += OnCollectionChanged;
    }

    public CustomObservableCollection(List<T> list) : base(list)
    {
        CollectionChanged += OnCollectionChanged;
    }

    public CustomObservableCollection(IEnumerable<T> collection) : base(collection)
    {
        CollectionChanged += OnCollectionChanged;
    }

    private void UnregisterItems(IList? items)
    {
        if (items is null) return;
        foreach (T item in items)
        {
            item.PropertyChanged -= EntityViewModelPropertyChanged;
        }
    }

    private void RegisterItems(IList? items)
    {
        if (items is null) return;
        foreach (T item in items)
        {
            item.PropertyChanged += EntityViewModelPropertyChanged;
        }
    }

    private void OnCollectionChanged(object? sender, NotifyCollectionChangedEventArgs e)
    {
        switch (e.Action)
        {
            case NotifyCollectionChangedAction.Remove:
                UnregisterItems(e.OldItems);
                break;

            case NotifyCollectionChangedAction.Add:
                RegisterItems(e.NewItems);
                break;

            case NotifyCollectionChangedAction.Reset:
                UnregisterItems(e.OldItems);
                RegisterItems(e.NewItems);
                break;

            case NotifyCollectionChangedAction.Move:
                break;

            case NotifyCollectionChangedAction.Replace:
                UnregisterItems(e.OldItems);
                RegisterItems(e.NewItems);
                break;
        }

        ItemsChanged?.Invoke(this, e);
    }

    public void EntityViewModelPropertyChanged(object? sender, PropertyChangedEventArgs e) => ItemsChanged?.Invoke(this, e);
}
```