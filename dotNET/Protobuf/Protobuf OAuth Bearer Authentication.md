Install the following NuGet package
```powershell
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```

## Validate the JWT Token on the Server

Register the services
```csharp
app.UseAuthentication();
app.UseAuthorization();
```

Add the JWT Bearer Authentication
```csharp
services
    .AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.Authority = "https://localhost:5001"; // address of the OAuth Server
        options.Audience = "grpc";
        options.RequireHttpsMetadata = false;

        if(env.IsDevelopment())
        {
	        options.TokenValidationParameters = new TokenValidationParameters
	        {
	            ValidateAudience = false;
	        };
        }
    });
```

Configure the authorization policy
```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("GrpcPolicy", policy =>
    {
        policy.RequireAuthenticatedUser();
        policy.ReqireClaim(/*...*/);
    });
});
```

Add the `Authorize` Attribute on the method of the GRPC method
```csharp
[Authorize(Policy = "GrpcPolicy")]
public override async Task<HelloReply> SayHello(HelloRequest request, ServerCallContext context)
{
    return await Task.FromResult(new HelloReply
    {
        Message = "Hello " + request.Name
    });
}
```

Alternatively, register the policy in the service mapping
```csharp
endpoints
    .MapGrpcService<MyGrpcService>()
    .RequireAuthorization("GrpcPolicy");
```

## Send the JWT token from the client

```csharp
var jwtToken = /**/;
var metadata = new Metadata
{
    { "Authorization": $"Bearer {jwtToken}" }
};
var request = new HelloRequest { /*...*/ };
var response = await client.SayHelloAsync(request, metadata);
```
