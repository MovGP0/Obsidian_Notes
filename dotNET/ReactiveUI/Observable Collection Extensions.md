```csharp
internal static class ObservableCollectionExtensions
{
    internal static void Apply<T, TKey>(this IObservableCollection<T> collection, IChangeSet<T, TKey> changes, Func<T, TKey> keySelector)
        where TKey : notnull
    {
        foreach (var change in changes)
        {
            switch (change.Reason)
            {
                case ChangeReason.Add:
                    collection.Add(change.Current);
                    break;
                case ChangeReason.Remove:
                    var itemToRemove = collection
                        .FirstOrDefault(o => Equals(keySelector(o), keySelector(change.Current)));
                    if (itemToRemove is { })
                    {
                        collection.Remove(itemToRemove);
                    }
                    break;
            }
        }
    }
}
```