```csharp
/// <summary>
/// Wraps an <see cref="IObserver{T}"/> for property change notifications, ensuring that only valid objects of type <typeparamref name="T"/> are passed to the observer.
/// </summary>
/// <typeparam name="T">The type of the object being observed.</typeparam>
public sealed class ObserverWrapper<T> : IObserver<object>
{
    private readonly IObserver<T> _innerObserver;

    /// <summary>
    /// Initializes a new instance of the <see cref="ObserverWrapper{T}"/> class.
    /// </summary>
    /// <param name="innerObserver">The inner observer to wrap.</param>
    public ObserverWrapper(IObserver<T> innerObserver) => _innerObserver = innerObserver;

    /// <summary>
    /// Notifies the observer that the provider has finished sending push-based notifications.
    /// </summary>
    public void OnCompleted() => _innerObserver.OnCompleted();

    /// <summary>
    /// Notifies the observer that the provider has experienced an error condition.
    /// </summary>
    /// <param name="error">An object that provides additional error information.</param>
    public void OnError(Exception error) => _innerObserver.OnError(error);

    /// <summary>
    /// Provides the observer with new data, passing the object only if it matches the expected type.
    /// </summary>
    /// <param name="value">The current notification information.</param>
    public void OnNext(object value)
    {
        if (value is T tValue)
        {
            _innerObserver.OnNext(tValue);
        }
    }
}
```
