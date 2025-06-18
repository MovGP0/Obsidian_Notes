## MCP Server using .NET

### Hosting in CLI Application

```bash
dotnet add package Microsoft.Extensions.Hosting
dotnet add package ModelContextProtocol --prerelease
```

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using ModelContextProtocol.Protocol;
using ModelContextProtocol.Server;
using System.ComponentModel;
using System.Threading.Tasks;
```

```csharp
async Task Main(string[] args)
{
    var builder = Host.CreateApplicationBuilder(args);

    builder.Logging
        .AddConsole(consoleLogOptions =>
        {
            consoleLogOptions.LogToStandardErrorThreshold = LogLevel.Trace;
        });

    var toolCollection = new McpServerPrimitiveCollection<McpServerTool>();

    builder.Services
        .AddMcpServer(opts =>
        {
            opts.KnownClientInfo = new()
            {
                Name = "MyClient",
                Version = "1.0.0"
            };

            opts.ServerInfo = new()
            {
                Name = "MyServer",
                Version = "1.0.0"
            };

            opts.Capabilities = new()
            {
                Tools = new ToolsCapability()
                {
                    ListChanged = true,
                    ToolCollection = toolCollection
                }
            };

            opts.ScopeRequests = true;
            opts.InitializationTimeout = TimeSpan.FromSeconds(10);
            opts.ServerInstructions = "Instructions for the client";
        })
        .WithStdioServerTransport();

 	var cts = new CancellationTokenSource();
    var task = builder.Build().RunAsync(cts.Token);

    // dynamically adding tools
    var toolOptions = new McpServerToolCreateOptions()
    {
        Name = nameof(Echo),
        Description = "Description for AI agents",
        Title = "Description for Humans",
        Idempotent = true,   // false => the tool has a side-effect on it's environment
        Destructive = false, // true => the tool may have a destructive side-effect on it's environment
        ReadOnly = false,    // true => the tool only performs read-only operations
        OpenWorld = false    // true => the tool may use external services (web search, other agents/tools, etc.)
    };
    toolCollection.Add(McpServerTool.Create(Echo, toolOptions));

    Console.Read();
	cts.Cancel();
}

// example tool for testing
private void Echo(string textParameter, int numberParameter)
{
    Console.WriteLine(textParameter + ": " + numberParameter);
}
```

## Hosing using ASP.NET Core

```bash
dotnet add package ModelContextProtocol.AspNetCore --prerelease
```

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services
    .AddMcpServer(opts =>
    {
        // ...
    })
    .WithHttpTransport(opts => {
        // ...
    });
```

## References

- [MCP C# SDK](https://github.com/modelcontextprotocol/csharp-sdk)
