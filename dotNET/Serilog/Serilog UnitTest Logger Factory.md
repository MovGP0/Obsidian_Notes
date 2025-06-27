```csharp
public static class LoggingFactory
{
	private const string OutputTemplate =
		"[{Timestamp:HH:mm:ss} {Level:u3} {SourceContext}]{NewLine}" +
		"in method {MemberName} at {FilePath}:{LineNumber}{NewLine}" +
		"{Message:lj}{NewLine}" +
		"{Exception}{NewLine}";

	public static ILogger CreateLogger()
	{
		return new LoggerConfiguration()
			.MinimumLevel.Verbose()
			.WriteTo.NUnitOutput(LogEventLevel.Verbose, outputTemplate: OutputTemplate)
			.WriteTo.Console(LogEventLevel.Verbose, OutputTemplate)
			.CreateLogger();
	}
}
```