Implement Request
```csharp
public sealed class Request : IRequest<string> { }
```

Implement handler
```csharp
public sealed class RequestHandler : IRequestHandler<Request, string>
{
    public async Task<string> Handle(Request request, CancellationToken cancellationToken)
    {
	    // do something
	    await Task.CompletedTask;
        return "Pong";
    }
}
```

Send request and get the response
```csharp
var response = await mediator.Send(new Request());
```