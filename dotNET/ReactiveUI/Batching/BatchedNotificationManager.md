```csharp
/// <summary>
/// Manages batched notifications for property changes on an object implementing <see cref="INotifyPropertyChanged"/>.
/// </summary>
[Entity]
[DebuggerDisplay("{BatchCount} batches active, {ChangedProperties.Count} changed properties, {Subscriptions.Count} subscriptions")]
public sealed class BatchedNotificationManager
{
    /// <summary>
    /// The number of active batches. When this value is greater than 0, notifications are aggregated.
    /// </summary>
    public int BatchCount { get; set; }

    /// <summary>
    /// The set of properties that have changed during the batch. These are processed when the batch ends.
    /// </summary>
    public HashSet<string> ChangedProperties { get; } = new();

    /// <summary>
    /// The list of active subscriptions for the object.
    /// </summary>
    public List<Subscription> Subscriptions { get; } = new();

    /// <summary>
    /// An object used to synchronize access to the manager's state.
    /// </summary>
    public readonly object Lock = new();
}
```