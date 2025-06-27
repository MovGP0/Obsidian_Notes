This pattern allows event-driven applications to demultiplex and dispatch service requests that are delivered to an application from one or more clients.

The Reactor pattern allows for efficient handling of multiple concurrent I/O events without blocking or creating a thread per connection. It leverages the event-driven architecture.

The [[Reactor Pattern]] uses a central event demultiplexer to monitor I/O events and dispatch them to event handlers, while the [[Proactor pattern]] initiates I/O operations asynchronously and uses completion handlers to process the results.

## Example

Implement the Reactor pattern using the `SocketAsyncEventArgs` class from the `System.Net.Sockets` namespace
```csharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

public class ReactorPatternExample
{
    private Socket listener;
    private const int BUFFER_SIZE = 1024;

    public async Task StartServerAsync()
    {
        // Create a listening socket
        listener = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
        listener.Bind(new IPEndPoint(IPAddress.Any, 1234));
        listener.Listen(10);

        Console.WriteLine("Server started. Listening for incoming connections...");

        // Start accepting connections asynchronously
        SocketAsyncEventArgs acceptEventArgs = new SocketAsyncEventArgs();
        acceptEventArgs.Completed += AcceptCompleted;
        await listener.AcceptAsync(acceptEventArgs);
    }

    private void AcceptCompleted(object sender, SocketAsyncEventArgs e)
    {
        if (e.SocketError == SocketError.Success)
        {
            Console.WriteLine("New connection accepted.");

            // Start receiving data asynchronously
            SocketAsyncEventArgs receiveEventArgs = new SocketAsyncEventArgs();
            receiveEventArgs.Completed += ReceiveCompleted;
            receiveEventArgs.SetBuffer(new byte[BUFFER_SIZE], 0, BUFFER_SIZE);
            receiveEventArgs.AcceptSocket = e.AcceptSocket;
            e.AcceptSocket.ReceiveAsync(receiveEventArgs);
        }
        else
        {
            Console.WriteLine($"Error accepting connection. SocketError: {e.SocketError}");
        }

        // Continue accepting connections asynchronously
        e.AcceptSocket = null;
        listener.AcceptAsync(e);
    }

    private void ReceiveCompleted(object sender, SocketAsyncEventArgs e)
    {
        if (e.SocketError == SocketError.Success && e.BytesTransferred > 0)
        {
            string receivedData = Encoding.ASCII.GetString(e.Buffer, e.Offset, e.BytesTransferred);
            Console.WriteLine($"Received data: {receivedData}");

            // Echo the received data back to the client
            byte[] sendData = Encoding.ASCII.GetBytes(receivedData);
            e.AcceptSocket.Send(sendData, 0, sendData.Length, SocketFlags.None);

            // Continue receiving data asynchronously
            e.SetBuffer(new byte[BUFFER_SIZE], 0, BUFFER_SIZE);
            e.AcceptSocket.ReceiveAsync(e);
        }
        else
        {
            Console.WriteLine($"Error receiving data. SocketError: {e.SocketError}");
        }
    }

    public static void Main()
    {
        ReactorPatternExample server = new ReactorPatternExample();
        server.StartServer();

        Console.ReadKey();
    }
}
```