## Guidelines

- Implement CQRS
	- Use [[MediatR Command Pattern]] for Events and Commands (Insert/Update)
	- Use [[MediatR Request-Response Pattern]] for Queries (Read)
- Use [[MediatR Pipeline Behavior]] for validation
	- Consider `MediatR.Extensions.FluentValidation`

## Configuration

```csharp
services.AddMediatR(cfg =>
{
    cfg.RegisterServicesFromAssembly(typeof(Startup).Assembly);
    cfg.AddBehavior<IPipelineBehavior<Ping, Pong>, PingPongBehavior>();
    cfg.AddOpenBehavior(typeof(GenericBehavior<,>));
});
```

Alternative
```csharp
services.AddMediatR(cfg =>
{
    cfg
	    .RegisterServicesFromAssemblyContaining<Program>()
	    .AddBehaviour<IPipelineBehavior<Ping, Pong>, PingPongBehavior>()
        .AddOpenBehavior(typeof(GenericBehavior<,>));
}
```
