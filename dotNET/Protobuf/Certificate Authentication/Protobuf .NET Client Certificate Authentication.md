Provide Certificate
```xml
<ItemGroup>
	<None Update="path/to/certificate.pfx">
	    <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
	</None>
</ItemGroup>
```

Use Certificate in GRPC channel
```csharp
var certificate = new X502Certificate("path/to/certificate.pfx", "password");
var handler = new HttpClientHandler();
handler.ClientCertificates.Add(certificate);

var options = new GrpcChannelOptions
{
    HttpHandler = handler
};

using var channel = GrpcChannel.ForAddress("...", options);
```
