```csharp
/// <summary>
/// Represents a subscription to property change notifications on a specific object and a set of property paths.
/// </summary>
[DebuggerDisplay("{DebuggerDisplay}")]
public sealed class Subscription : IDisposable
{
    private readonly object _obj;
    public List<string> PropertyPaths { get; }
    public IObserver<object> Observer { get; }
    private readonly BatchedNotificationManager _manager;
    private readonly List<PropertyPathSubscription> _pathSubscriptions = new();

    /// <summary>
    /// Initializes a new instance of the <see cref="Subscription"/> class.
    /// </summary>
    /// <param name="obj">The object whose properties are being observed.</param>
    /// <param name="propertyPaths">The property paths to observe.</param>
    /// <param name="observer">The observer that will be notified of property changes.</param>
    /// <param name="manager">The manager responsible for batching notifications.</param>
    public Subscription(
        object obj,
        List<string> propertyPaths,
        IObserver<object> observer,
        BatchedNotificationManager manager)
    {
        _obj = obj ?? throw new ArgumentNullException(nameof(obj));
        PropertyPaths = propertyPaths ?? throw new ArgumentNullException(nameof(propertyPaths));
        Observer = observer ?? throw new ArgumentNullException(nameof(observer));
        _manager = manager ?? throw new ArgumentNullException(nameof(manager));

        foreach (var path in propertyPaths)
        {
            var pathSubscription = new PropertyPathSubscription(obj, path, () => HandlePropertyPathChanged(path), manager);
            _pathSubscriptions.Add(pathSubscription);
        }
    }

    private void HandlePropertyPathChanged(string path)
    {
        lock (_manager.Lock)
        {
            if (_manager.BatchCount > 0)
            {
                _manager.ChangedProperties.Add(path);
                return;
            }
        }

        // notify if there is no active batch
        Observer.OnNext(_obj);
    }

    /// <summary>
    /// Disposes of the subscription, detaching from the observed object's property change events.
    /// </summary>
    public void Dispose()
    {
        foreach (var pathSubscription in _pathSubscriptions)
        {
            pathSubscription.Dispose();
        }
    }

    public string DebuggerDisplay
    {
        get
        {
            var subscribedObjectType = _obj.GetType().Name;
            return $"Subscribed {string.Join(".", PropertyPaths)} of type {subscribedObjectType}";
        }
    }
}
```