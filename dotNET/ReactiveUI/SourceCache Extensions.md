```csharp
using ReactiveUI;

namespace DynamicData.Binding;

public static class SourceCacheExtensions
{
    public static void Update<T, TKey>(this SourceCache<T, TKey> cache, ICollection<T> items, Func<T, TKey> getKey, bool dispose = false)
        where TKey : notnull
    {
        var itemKeys = items.Select(getKey).ToArray();
        var keysToRemove = cache.Keys.Where(e => !itemKeys.Contains(e)).ToArray();

        if (dispose)
        {
            var itemsToDispose = cache.Items
                .Where(item => keysToRemove.Contains(getKey(item)));

            foreach (var item in itemsToDispose)
            {
                DisposeItem(item);
            }
        }

        cache.RemoveKeys(keysToRemove);
        cache.AddOrUpdate(items);
    }

    private static void DisposeItem<T>(T item)
    {
        if (item is IActivatableViewModel activatableViewModel)
        {
            activatableViewModel.Activator.Deactivate();
            activatableViewModel.Activator.Dispose();
        }

        if (item is IDisposable disposable)
        {
            disposable.Dispose();
        }
    }
}
```