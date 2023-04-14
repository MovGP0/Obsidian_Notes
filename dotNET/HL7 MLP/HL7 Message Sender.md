```csharp
public interface IHl7MessageSender
{
	Task SendMessageAsync(string domain, int port, string message, CancellationToken cancellationToken);
	Task SendMessageAsync(IPAddress address, int port, string message, CancellationToken cancellationToken);
	Task SendMessageAsync(IPEndPoint endpoint, string message, CancellationToken cancellationToken);
}

public sealed class Hl7MessageSender : IHl7MessageSender
{
	private static ILogger Log => Serilog.Log.ForContext<Hl7MessageReceiver>();

	public async Task SendMessageAsync(string domain, int port, string message, CancellationToken cancellationToken)
	{
		var addresses = await Dns.GetHostAddressesAsync(domain);

		if (addresses.Length == 0)
		{
			throw new InvalidOperationException($"The domain {domain} could not be resolved to an valid IP address.");
		}

		var address = addresses.First();
		await SendMessageAsync(new IPEndPoint(address, port), message, cancellationToken);
	}

	public async Task SendMessageAsync(IPAddress address, int port, string message, CancellationToken cancellationToken)
	{
		await SendMessageAsync(new IPEndPoint(address, port), message, cancellationToken);
	}

	public async Task SendMessageAsync(IPEndPoint endpoint, string message, CancellationToken cancellationToken)
	{
		Log.Verbose($"Sending message to {endpoint}");

		var sanitizedMessage = message
			.Replace(SpecialCharacters.CarriageReturn.ToString(), string.Empty)
			.Replace(SpecialCharacters.EndOfBlock.ToString(), string.Empty)
			.Replace(SpecialCharacters.StartOfBlock.ToString(), string.Empty);

		var hl7Data = Encoding.UTF8.GetBytes(sanitizedMessage);
		var dataLength = hl7Data.Length;
		var dataToSend = new byte[dataLength + 3];
		dataToSend[0] = (byte)SpecialCharacters.StartOfBlock;
		Array.Copy(hl7Data, 0, dataToSend, 1, dataLength);
		dataToSend[dataLength + 1] = (byte)SpecialCharacters.EndOfBlock; 
		dataToSend[dataLength + 2] = (byte)SpecialCharacters.CarriageReturn;

		using var sender = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
		sender.Connect(endpoint);
		sender.SendBufferSize = 4096;
		await sender.SendAsync(dataToSend, SocketFlags.None, cancellationToken);
		sender.Disconnect(false);
	}
}
```