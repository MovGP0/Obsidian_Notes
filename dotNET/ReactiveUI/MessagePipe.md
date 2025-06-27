## Setup

```powershell
Install-Package MessagePipe
```

```csharp
services.AddMessagePipe();
```

## Publish/Subscribe

MessagePipe (sync)
```csharp
var publisher = serviceProvider.GetRequiredService<IPublisher<MyMessage>>();
var subscriber = serviceProvider.GetRequiredService<ISubscriber<MyMessage>>();

subscriber
	.Subscribe(message => Console.WriteLine(message))
	.DisposeWith(disposables);

publisher.Publish(myMessage);
```

MessagePipe (async)
```csharp
var asyncPublisher = serviceProvider.GetRequiredService<IAsyncPublisher<MyMessage>>();
var asyncSubscriber = serviceProvider.GetRequiredService<IAsyncSubscriber<MyMessage>>();

asyncSubscriber
	.Subscribe(async (message, cancellationToken) =>
	{
	    await // ...
	})
	.DisposeWith(disposables);

await asyncPublisher.PublishAsync(myMessage, cancellationToken);
```

MediatR
```csharp
public class NotificationMessage : INotification
{
    public string Message { get; set; }
}

public class MessageHandler : INotificationHandler<NotificationMessage>
{
    public Task Handle(NotificationMessage notification, CancellationToken cancellationToken)
    {
        Console.WriteLine(notification.Message);
        return Task.CompletedTask;
    }
}

await mediator.Publish(new NotificationMessage { Message = "Hello, MediatR!" });
```

## Request/Response

MessagePipe (sync)
```csharp
public sealed class MyRequestHandler : IRequestHandler<MyRequest, MyResponse>
{
    public MyResponse Handle(MyRequest request)
    {
        // ...
    }
}
```

```csharp
var handler = serviceProvider.GetRequiredService<IRequestHandler<MyRequest, MyResponse>>();
MyResponse response = handler.Invoke(new MyRequest());
```

MessagePipe (async)
```csharp
public class AsyncRequestHandler : IAsyncRequestHandler<MyRequest, MyResponse>
{
    public Task<MyResponse> InvokeAsync(MyRequest request, CancellationToken cancellationToken)
    {
	    // ...
        return Task.FromResult(myResponse);
    }
}

var asyncHandler = serviceProvider.GetRequiredService<IAsyncRequestHandler<MyRequest, MyResponse>>();
var response = await asyncHandler.InvokeAsync(myRequest, CancellationToken.None);
```

MediatR
```csharp
public class MyRequestHandler : IRequestHandler<MyRequest, MyResponse>
{
    public Task<MyResponse> Handle(MyRequest request, CancellationToken cancellationToken)
    {
        // ...
	    return Task.FromResult(myResponse);
    }
}

var response = await mediator.Send(myRequest);
```

## References

- [GitHub: MessagePipe](https://github.com/Cysharp/MessagePipe)
- [NuGet: MessagePipe](https://www.nuget.org/packages/MessagePipe)