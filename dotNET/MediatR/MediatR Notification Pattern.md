Implement notification
```csharp
public sealed class Notification : INotification { }
```

Implement notification handlers
```csharp
public sealed class NotificationHandler : INotificationHandler<Notification>
{
    public async Task Handle(Notification notification, CancellationToken cancellationToken)
    {
        // do something
        return await Task.CompletedTask;
    }
}

// Optional: add additional notification handlers
```

Send notification
```csharp
await mediator.Publish(new Notification());
```
