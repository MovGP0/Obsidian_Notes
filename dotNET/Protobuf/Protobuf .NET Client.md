Example `.proto` File:
```protobuf
service ServiceName {
  rpc GetPerson(int32 personId) returns (Person);
  rpc SendPeople(stream Person) returns SuccessMessage;
  rpc GetPeople() returns (stream Person);
  rpc ProcessPeople(stream Person) returns (stream Person);
}
```

## .NET Client

Include Protobuf files
```xml
<ItemGroup>
    <Protobuf Include="Protos\foobar.proto" GrpcServices="Server" />
</ItemGroup>
```

Reference packages
```xml
<ItemGroup>
	<PackageReference Include="Google.Protobuf" Version="..." />
	<PackageReference Include="Grpc.Net.Client" Version="..." />
	<PackageReference Include="Grpc.Tools" Version="...">
	    <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
	    <PrivateAssets>all</PrivateAssets>
	</PackageReference>
</ItemGroup>
```

There will be a concrete type implemented for client
```csharp
using var channel = GrpcChannel.ForAddress("https://localhost:7042");
var client = new Solution.Project.ServiceNameClient(channel);
```

Async Request
```csharp
var response = client.GetPersonAsync(id, cancellationToken);
```

Send stream
```csharp
using var call = client.SendPeople();
foreach (var person in people)
{
    await call.RequestStream.WriteAsync(person);
}
await call.RequestStream.CompleteAsync();
```

Receive stream
```csharp
using var call = client.GetPeople();
while (await call.ResponseStream.MoveNext())
{
    var person = call.ResponseStream.Current;
    // ...
}
```

Process stream
```csharp
using var call = client.ProcessPeople();

var readTask = Task.Run(() => {
	while (await call.ResponseStream.MoveNext())
	{
	    var person = call.ResponseStream.Current;
	    // ...
	}
});

var writeTask = Task.Run(() => {
	foreach (var person in people)
	{
	    await call.RequestStream.WriteAsync(person);
	}
	await call.RequestStream.CompleteAsync();
});

Task.WaitAll(readTask, writeTask);
```
