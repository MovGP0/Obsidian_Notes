Example `.proto` File:
```protobuf
service ServiceName {
  rpc GetPerson(int32 personId) returns (Person);
  rpc SendPeople(stream Person) returns SuccessMessage;
  rpc GetPeople() returns (stream Person);
  rpc ProcessPeople(stream Person) returns (stream Person);
}
```

## ASP.NET Server

Include Protobuf files
```xml
<ItemGroup>
    <Protobuf Include="Protos\foobar.proto" GrpcServices="Server" />
</ItemGroup>
```

Reference `Grpc.AspNetCore`
```xml
<ItemGroup>
    <PackageReference Include="Grpc.AspNetCore" Version="..." />
</ItemGroup>
```

Configure Kestrel for HTTP/2
```csharp
webBuilder.ConfigureKestrel(options =>
{
    options.ListenLocalhost(442, opts =>
    {
        opts.Protocols = HttpProtocol.Http2;
    });
});
```

Setup
```csharp
services.AddGrpc();
services.UseCors();
```

```csharp
endpoints.MapGrpcService<ServiceName>();
```

Configure CORS (for Blazor client)
```csharp
services.AddCors(o => o.AddPolicy("AllowAnyGrpcWeb", builder => {
    builder
        .AllowOrigin("foobar.com")
        .AllowAnyMethod()
        .AllowAnyHeader()
        .WithExposedHeaders("Grpc-Status", "Grpc-Message", "Grpc-Encoding", "Grpc-Accept-Encoding");
}))
```

Implement base class for service
```csharp
using Solution.Project;

public sealed class MyService : ServiceName
{
    private ILogger<MyService> Log { get; }

    public MyService(ILogger<MyService> log) : base(
    {
         Log = log;
    }

    public override async Task<Person> GetPerson(int id)
    {
        // ...
    }

    public override async Task<SuccessMessage> SendPeople(
	    IAsyncStreamReader<Person> requestStream,
	    ServerCallContext context)
    {
	    while (await requestStream.MoveNext())
	    {
		    var person = requestStream.Current;
		    // ...
	    }
    }

    public override async Task GetPeople(
        IServerStreamWriter<Person> responseStream,
        ServerCallContext context)
    {
        // ...
        foreach (var person in people)
        {
            await responseStream.WriteAsync(person);
        }
    }

    public override async Task ProcessPeople(
        IAsyncStreamReader<Person> requestStream,
        IServerStreamWriter<Person> responseStream,
        ServerCallContext context)
    {
        while (await requestStream.MoveNext())
	    {
		    var person = requestStream.Current;
		    // ...
		    await responseStream.WriteAsync(person);
	    }
    }
}
```
