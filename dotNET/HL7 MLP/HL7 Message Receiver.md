```csharp
public interface IHl7MessageReceiver
{
	IObservable<string> Observe(IPEndPoint endPoint);
	Task<IObservable<string>> ObserveAsync(string domain, int port);
	IObservable<string> Observe(IPAddress address, int port);
}

public sealed class Hl7MessageReceiver : IHl7MessageReceiver
{
	private static ILogger Log => Serilog.Log.ForContext<Hl7MessageReceiver>();

	public IObservable<string> Observe(IPEndPoint endPoint)
	{
		var cts = new CancellationTokenSource();
		return Observable.Create<string>(observer =>
		{
			_ = Task.Run(async () =>
			{
				Log.Verbose($"Starting Receive on {endPoint}");
				try
				{
					await ReceiveAsync(observer, endPoint, cts.Token);
				}
				catch (OperationCanceledException)
				{
					Log.Verbose($"Stopping Receive on {endPoint}");
				}
				catch (Exception e)
				{
					Log.Error(e, e.Message);
				}
			}, cts.Token);

			return Disposable.Create(() => cts.Cancel());
		});
	}

	public async Task<IObservable<string>> ObserveAsync(string domain, int port)
	{
		var addresses = await Dns.GetHostAddressesAsync(domain);

		if (addresses.Length == 0)
		{
			throw new InvalidOperationException($"The domain {domain} could not be resolved to an valid IP address.");
		}

		var address = addresses.First();
		return Observe(new IPEndPoint(address, port));
	}

	public IObservable<string> Observe(IPAddress address, int port)
	{
		return Observe(new IPEndPoint(address, port));
	}

	private static async Task ReceiveAsync(IObserver<string> observer, EndPoint endpoint, CancellationToken cancellationToken)
	{
		try
		{
			var buffer = new byte[1024];
			var builder = new StringBuilder(buffer.Length);

			using var listener = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
			listener.Bind(endpoint);
			listener.Listen(3);
			using var receiver = listener.Accept();

			while (!cancellationToken.IsCancellationRequested)
			{
				var count = await receiver.ReceiveAsync(buffer, SocketFlags.None, cancellationToken);
				if (count == 0)
				{
					builder.Clear();
					Thread.Sleep(100);
					continue;
				}

				Log.Verbose($"{count} bytes received");

				var data = Encoding.UTF8.GetString(buffer, 0, count);
				builder.Append(data);

				var end = data.IndexOf(SpecialCharacters.CarriageReturn);
				if (end < 0) continue;
				
				var messages = builder
					.ToString()
					.Split(SpecialCharacters.CarriageReturn);

				builder.Clear();

				foreach (var message in messages)
				{
					if (string.IsNullOrWhiteSpace(message)) continue;

					var cleanedMessage = message
						.Replace(SpecialCharacters.StartOfBlock.ToString(), string.Empty)
						.Replace(SpecialCharacters.EndOfBlock.ToString(), string.Empty)
						.Replace(SpecialCharacters.CarriageReturn.ToString(), string.Empty);

					if (string.IsNullOrWhiteSpace(cleanedMessage)) continue;

					observer.OnNext(cleanedMessage);
				}
			}
		}
		catch (SocketException ex)
		{
			Log.Error(ex, ex.Message);
			observer.OnError(ex);
		}
	}
}
```