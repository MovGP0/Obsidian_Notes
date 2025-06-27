## MCP Client using .NET

### Accessing CLI Tool

```csharp
using ModelContextProtocol.Client;
using ModelContextProtocol.Transport;

var cliToolOptions = new StdioClientTransportOptions
{
    Name = "my-mcp-cli",
    Command = "dotnet",
    Arguments = new[] { "run", "--stdio" }
};

var clientTransport = new StdioClientTransport(cliToolOptions);

using var mcp = await McpClientFactory.CreateAsync(clientTransport);
await mcp.InitializeAsync();

// Discover available tools:
var tools = await mcp.ListToolsAsync();

// Call a tool:
var result = await mcp.InvokeToolAsync<string>(
    toolName: "Echo",
    argsJson: JsonSerializer.Serialize(new { textParameter = "Hello", numberParameter = 1 })
);

// Optionally, listen for server notifications:
mcp.OnNotification += (n) => Console.WriteLine($"Notification: {n.Method}");
```

### Accessing Web Tool

```csharp
using ModelContextProtocol.Client;
using ModelContextProtocol.Client.Http;

var httpOptions = new HttpClientTransportOptions
{
    Endpoint = new Uri("http://localhost:5000/mcp")
};

using var mcp = await McpClientFactory.CreateAsync(httpOptions);
await mcp.InitializeAsync();

var toolList = await mcp.ListToolsAsync();
var echoResult = await mcp.InvokeToolAsync<string>(
    toolName: "Echo",
    argsJson: JsonSerializer.Serialize(new { message = "Hi, HTTP!" })
);

Console.WriteLine(echoResult);
```
