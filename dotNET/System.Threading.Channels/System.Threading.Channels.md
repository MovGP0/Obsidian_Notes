```csharp
using System.Threading.Channels;

public sealed class InMemoryMessageQueue<T>
{
	private readonly Channel<T> _channel = Channel.CreateUnbounded<T>();
	public ChannelReader<T> Reader => _channel.Reader;
	public ChannelWriter<T> Writer => _channel.Writer;
}
```

```csharp
builder.Services.AddSingleton<InMemoryMessageQueue<MyEvent>>();
```

Write message:
```csharp
writer.TryWrite(message);
```

Get message:
```csharp
ValueTask<T> messageTask = reader.ReadAsync();
T message = await messageTask;
```

## References

- [Milan Jovanović: Lightweight In-Memory Message Bus Using .NET Channels](https://www.milanjovanovic.tech/blog/lightweight-in-memory-message-bus-using-dotnet-channels)
- [Stephen Toub: An Introduction to System.Threading.Channels](https://devblogs.microsoft.com/dotnet/an-introduction-to-system-threading-channels/)