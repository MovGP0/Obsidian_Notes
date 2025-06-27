```csharp
public sealed class GrpcServiceWrapper : IGrpcServiceWrapper
{
    private IServiceProvider ServiceProvider { get; }
    private List<GrpcChannel> RoundRobinChannels { get; } = new()

    public GrpcServiceWrapper(
	    IConfiguration configuration,
	    IServiceProvider serviceProvider)
    {
	    ServiceProvider = serviceProvider;
	    var addresses = configuration.GetSection("GrpcServers").Get<List<string>>();
	    foreach(var address in addresses)
	    {
		    var channel = GrpcChannel.ForAddress(address);
		    RoundRobinChannels.Add(channel);
	    }
    }
}
```

```csharp
services.AddSingleton<IConfiguration>(Configuration);
services.AddSingleton<IGrpcServiceWrapper, GrpcServiceWrapper>();
```