Configuration
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
	    .AddBehaviour<IPipelineBehavior<Ping, Pong>, PingPongBehavior>();
}
```

