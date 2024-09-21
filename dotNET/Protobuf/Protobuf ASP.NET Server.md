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
using Microsoft.AspNetCore.Server.Kestrel.Core;

public static IHostBuilder CreateHostBuilder(sting[] args)
{
	Host
		.CreateDefaultBuilder(args)
		.ConfigureWebHostDefaults(webBuilder =>
		{	
			webBuilder.ConfigureKestrel(options =>
			{
			    options.ListenLocalhost(442, opts =>
			    {
			        opts.Protocols = HttpProtocols.Http2;
			    });
	
				options.ListenLocalhost(80, opts =>
				{
					opts.Protocols = HttpProtocols.Http1;
				});

				options.ConfigureHttpsDefaults(opts =>
				{
				    opts.ServerCertificate = new X502Certificate("path/to/certificate.pfx", "password");
				});
			});
	});
}
```

Setup
```csharp
hostBuider.ConfigureServices(services =>  
{  
    services.AddGrpc();
    services.AddCors();
    // ...
}
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

    public override async Task<Person> GetPerson(int id, ServerCallContext context)
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

## Expose .proto files via controller

Configure HTTPs Redirection
```csharp
app.UseHttpsRedirection();
```

```csharp
services.AddHttpsRedirection(options =>
{
	options.RedirectStatusCode = (int)HttpStatusCode.PermanentRedirect;
	options.HttpsPort = 443;
});
```

Create Controller that exposes `.proto` files on GET request.
```csharp
services.AddRouting();
```
```csharp
app.UseRouting();
app.UseEndpoints(endpoints =>
{
    endpoints.MapGet("/proto", async context =>
    {
        var requestPath = $"{context.Request.Scheme}://{context.Request.Host}:{context.Request.Port}{context.Request.Path}/";
        var directory = Server.MapPath("~/proto");
        string[] protoFiles = Directory
            .GetFiles(directory, "*.proto")
			.Select(Path.GetFileName)
			.Select(fileName =>
			{
				var uri = new UriBuilder(requestPath);
				uri.Path += fileName;
				return uri.ToString()
			})
			.ToArray();

        await context.Response.WriteAsJsonAsync(protoFiles);
    });

	endpoints.MapGet("/proto/{filename}", async context =>
	{
		//var fileName = context.Request.Query["filename"];
		var fileName = context.Request.RouteValues["filename"].ToString();
		if(!fileName.EndsWith(".proto"))
		{
		    context.Response.StatusCode = 404;
		    return;
		}

        var filePath = Path.Combine(Server.MapPath("~/proto"), fileName);
		if(!File.Exists(filePath))
		{
		    context.Response.StatusCode = 404;
		    return;
		}

		var fileStream = File.OpenRead(filePath);
		context.Response.ContentType = "application/protobuf";
		await fileStream.CopyToAsync(context.Response.Body);
		await context.Response.Body.FlushAsync();
	});
});
```