DNS Server returns a list of the GRPC server instances

```csharp
var options = new GrpcChannelOptions
{
    Credentials = ChannelCredentials.SecureSsl,
    ServiceProvider = serviceProvider,
    ServiceConfig = new()
    {
        LoadBalancingOptions = {
            new PickFirstConfig()
        }
    }
};

using var channel = GrpcChannel.ForAddress("dns://servername", options);

var client = new SomeGrpcClient(channel);
```
