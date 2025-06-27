```csharp
public sealed class PropertyPathSubscription : IDisposable
{
    private readonly string _propertyPath;
    private readonly Action _onPropertyChanged;
    private readonly BatchedNotificationManager _manager;
    private readonly List<IDisposable?> _subscriptions = new();
    private readonly object _lock = new();
    private readonly object _rootObject;
    private readonly ILogger _log;

    public PropertyPathSubscription(
        object rootObject,
        string propertyPath,
        Action onPropertyChanged,
        BatchedNotificationManager manager)
    {
        _log = ServiceLocator.GetLogger<PropertyPathSubscription>();
        _log.LogTrace("Initializing PropertyPathSubscription for path '{Path}'", propertyPath);

        _rootObject = rootObject ?? throw new ArgumentNullException(nameof(rootObject));
        _propertyPath = propertyPath ?? throw new ArgumentNullException(nameof(propertyPath));
        _onPropertyChanged = onPropertyChanged ?? throw new ArgumentNullException(nameof(onPropertyChanged));
        _manager = manager ?? throw new ArgumentNullException(nameof(manager));

        var pathParts = propertyPath.Split('.');
        SubscribeToPath(rootObject, pathParts, 0);
    }

    private void SubscribeToPath(object? currentObject, string[] path, int index)
    {
        if (currentObject == null || index >= path.Length)
        {
            return;
        }

        _log.LogTrace("Subscribing to path '{Path}' at index {Index}", _propertyPath, index);

        string propertyName = path[index];
        _log.LogTrace("Subscribing to property '{PropertyName}'", propertyName);

        if (currentObject is INotifyPropertyChanged inpc)
        {
            PropertyChangedEventHandler handler = (sender, e) =>
            {
                _log.LogTrace("PropertyChanged event triggered for property '{Property}'", e.PropertyName);

                if (e.PropertyName == propertyName || string.IsNullOrEmpty(e.PropertyName))
                {
                    UnsubscribeFromIndex(index + 1);

                    object? newValue = GetPropertyValue(_rootObject, path, 0, index + 1);
                    SubscribeToPath(newValue, path, index + 1);

                    string propertyChain = string.Join(".", path.Take(index + 1));
                    _log.LogTrace("Notifying observer that property '{Property}' has changed", propertyChain);
                    NotifyPropertyChanged();
                }
            };

            inpc.PropertyChanged += handler;
            _log.LogTrace("Handler subscribed to property '{PropertyName}' at index {Index}", propertyName, index);

            lock (_lock)
            {
                // Ensure subscriptions are aligned with the path index
                while (_subscriptions.Count <= index)
                {
                    _subscriptions.Add(null);
                }

                var subscription = Disposable.Create(() => inpc.PropertyChanged -= handler);
                _subscriptions[index] = subscription;
            }
        }

        // Get the next object in the path
        object? nextObject = GetPropertyValue(currentObject, path, index, 1);
        SubscribeToPath(nextObject, path, index + 1);
    }

    private void UnsubscribeFromIndex(int index)
    {
        lock (_lock)
        {
            _log.LogTrace("Unsubscribing from index {Index} onwards", index);

            for (int i = _subscriptions.Count - 1; i >= index; i--)
            {
                _subscriptions[i]?.Dispose();
                _subscriptions.RemoveAt(i);
            }
        }
    }

    private object? GetPropertyValue(object? obj, string[] path, int startIndex, int length)
    {
        if (obj == null || startIndex >= path.Length)
            return null;

        string propertyChain = string.Join(".", path.Skip(startIndex).Take(length));
        _log.LogTrace("Retrieving value for property '{PropertyChain}'", propertyChain);
        return obj.GetPropertyValueFromPropertyChain(propertyChain);
    }

    private void NotifyPropertyChanged()
    {
        bool shouldNotify = false;

        lock (_manager.Lock)
        {
            if (_manager.BatchCount > 0)
            {
                _log.LogTrace("Batch is active. Adding property '{Path}' to ChangedProperties", _propertyPath);
                _manager.ChangedProperties.Add(_propertyPath);
            }
            else
            {
                shouldNotify = true;
            }
        }

        if (shouldNotify)
        {
            _log.LogTrace("No active batch. Invoking property changed notification");
            _onPropertyChanged();
        }
    }

    public void Dispose()
    {
        _log.LogTrace("Disposing PropertyPathSubscription for path '{Path}'", _propertyPath);
        UnsubscribeFromIndex(0);
    }
}
```