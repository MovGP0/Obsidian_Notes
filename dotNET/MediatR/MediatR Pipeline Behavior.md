### Implement Pipeline Step

```csharp
public sealed class MeasureCreateMovieBehavior
    : IPipelineBehavior<CreateMovieCommand, Result<Movie, ValidationFailed>>
{
    private readonly ILogger<MeasureCreateMovieBehavior> _logger;

    public MeasureCreateMovieBehavior(ILogger<MeasureCreateMovieBehavior> logger)
    {
	    _logger = logger;
    }

	public async Task<Result<Movie, ValidationFailed>> Handle (
		CreateMovieCommand request,
		RequestHandleDelegate<Result<Movie, ValidationFailed>> next,
		CancellationToken cancellationToken
	)
	{
		var start = Stopwatch.GetTimestamp();
	    var result = await next();
		var delta = Stopwatch.GetTimestamp();
		_logger.LogInformation("CreateMovieCommand {Status} in {Milliseconds} ms", result.IsSuccess ? "Succeded" : "Failed", deta.TotalMilliseconds);
		return result;
	}
}
```

```csharp
cfg.AddBehaviour<IPipelineBehavior<CreateMovieCommand, Result<Movie, ValidationFailed>>, MeasureCreateMovieBehavior>();
```

### Using Generics

```csharp
public sealed class MeasureBehavior<TCommand, TResult>
    : IPipelineBehavior<TCommand, TResult>
{
    private readonly ILogger<MeasureBehavior> _logger;

    public MeasureBehavior(ILogger<MeasureBehavior> logger)
    {
	    _logger = logger;
    }

	public async Task<TResult> Handle (
		CreateMovieCommand request,
		RequestHandleDelegate<TResult> next,
		CancellationToken cancellationToken
	)
	{
		var start = Stopwatch.GetTimestamp();
	    var result = await next();
		var delta = Stopwatch.GetTimestamp();
		_logger.LogInformation("{Command} {Status} in {Milliseconds} ms", typeof(TCommand), result.IsSuccess ? "Succeded" : "Failed", deta.TotalMilliseconds);
		return result;
	}
}
```

```csharp
cfg.AddOpenBehavior(typeof(MeasureBehavior<,>));
```
