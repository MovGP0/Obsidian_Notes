```csharp
using Grpc.Net.Client.Balancer;
using System.Net;

var endpoints = Configuration
	.GetSection("GrpcServers")
	.Get<List<string>>()
	.Select(addr => new Uri(addr))
	.Select(uri => new DnsEndPoint(uri.Host, uri.Port)))
	.ToArray();

var factory = new StaticResolverFactory(_ => endpoints)

services.AddSingleton<ResolverFactory>(factory);
```

```csharp
var options = new GrpcChannelOptions
{
	Credentials = ChannelCredentials.SecureSsl,
    ServiceProvider = serviceProvider,
    ServiceConfig = new()
    {
        LoadBalancingOptions = {
            new RoundRobinConfig()
        }
    }
};

using var channel = GrpcChannel.ForAddress("static://localhost", options);
```

Implement `Grpc.Net.Client.Balancer.Resolver` base class for custom resolver strategies.