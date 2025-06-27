Implement command
```csharp
public sealed class Command : IRequest<string> { }
```

Implement handler
```csharp
public sealed class CommandHandler : IRequestHandler<Command>
{
    public async Task Handle(Command request, CancellationToken cancellationToken)
    {
	    // do something
        await Task.CompletedTask;
    }
}
```

Send command
```csharp
await mediator.Send(new Command());
```