## HttpClient

```csharp
using System.Net.Http;
using System.Net.Http.Headers;

namespace SSEExample;

class Program
{
	static async Task Main(string[] args)
	{
		using var cancellationTokenSource = new CancellationTokenSource();
		Console.CancelKeyPress += (_, e) =>
		{
			e.Cancel = true;
			cancellationTokenSource.Cancel();
		};

		try
		{
			await ListenForEventsAsync(cancellationTokenSource.Token);
		}
		catch (OperationCanceledException)
		{
			Console.WriteLine("Event listening canceled.");
		}
	}

	static async Task ListenForEventsAsync(CancellationToken cancellationToken)
	{
		var client = new HttpClient(); // HttpClient should be dependency injected from pool
		var request = new HttpRequestMessage(HttpMethod.Get, "http://example.com/events");
		request.Headers.Accept.Add(new MediaTypeWithQualityHeaderValue("text/event-stream"));

		using var response = await client.SendAsync(request,
			HttpCompletionOption.ResponseHeadersRead,
			cancellationToken);
		response.EnsureSuccessStatusCode();

		using var stream = await response.Content.ReadAsStreamAsync();
		using var reader = new StreamReader(stream);
		using var eventSource = new EventSource(reader);

		eventSource.MessageReceived += (sender, e) =>
		{
			Console.WriteLine($"Event received: {e.EventType} - {e.Data}");
		};

		await eventSource.OpenAsync(cancellationToken);

		await cancellationToken.AsTask();
		await eventSource.CloseAsync();
	}
}
```

## ASP.NET Middleware

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.DependencyInjection;

namespace SSEExample;

public sealed class SSEMiddleware
{
	private readonly RequestDelegate _next;

	public SSEMiddleware(RequestDelegate next)
	{
		_next = next;
	}

	public async Task Invoke(HttpContext context)
	{
		if (context.Request.Path == "/events")
		{
			context.Response.Headers.Add("Content-Type", "text/event-stream");
			context.Response.Headers.Add("Cache-Control", "no-cache");

			var cancellationToken = context.RequestAborted;

			var eventService = context.RequestServices.GetRequiredService<EventService>();

			while (!cancellationToken.IsCancellationRequested)
			{
				var eventData = await eventService.GetNextEventAsync(cancellationToken);
				if (eventData != null)
				{
					var data = $"event: {eventData.EventType}\n";
					data += $"data: {eventData.Data}\n\n";

					await context.Response.WriteAsync(data, cancellationToken);
					await context.Response.Body.FlushAsync();
				}

				// Wait for a short period before checking again for new events.
				await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
			}

			return;
		}

		await _next(context);
	}
}
```

```csharp
public sealed class EventService
{
	private readonly SemaphoreSlim _eventSemaphore = new SemaphoreSlim(1);
	private readonly Queue<EventData> _eventQueue = new Queue<EventData>();

	public async Task AddEventAsync(string eventType, string eventData)
	{
		await _eventSemaphore.WaitAsync();
		try
		{
			_eventQueue.Enqueue(new EventData { EventType = eventType, Data = eventData });
		}
		finally
		{
			_eventSemaphore.Release();
		}
	}

	public async Task<EventData?> GetNextEventAsync(CancellationToken cancellationToken)
	{
		while (!cancellationToken.IsCancellationRequested)
		{
			await _eventSemaphore.WaitAsync(cancellationToken);

			try
			{
				if (_eventQueue.Count > 0)
				{
					return _eventQueue.Dequeue();
				}
			}
			finally
			{
				_eventSemaphore.Release();
			}

			// Wait for a short period before checking again for new events.
			await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
		}

		return null;
	}
}
```

```csharp
public struct EventData
{
	public required string EventType { get; init; }
	public required string Data { get; init; }
}
```

## References

- [Server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)