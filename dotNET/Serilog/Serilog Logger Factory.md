```csharp
public static class LoggingFactory
{
	private const string OutputTemplate =
		"[{Timestamp:HH:mm:ss} {Level:u3} {SourceContext}] [{EnvironmentUserName} {MachineName}]{NewLine}" +
		"in method {MemberName} at {FilePath}:{LineNumber}{NewLine}" +
		"{Message:lj}{NewLine}" +
		"{Exception}{NewLine}";

	public static ILogger CreateLogger(IConfiguration configuration)
	{
		if(configuration == null) throw new ArgumentNullException(nameof(configuration));

		var environment = Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT");

		var formatter = new CustomDateFormatter("yyyy-MM-dd", CultureInfo.InvariantCulture);
		var loggingConfig = configuration.GetSection("Logging");

		var dbCommandLogLevel = environment == "Development" || environment == "Staging" ? LogEventLevel.Warning : LogEventLevel.Fatal;

		var logConfig = new LoggerConfiguration()
			.MinimumLevel.Information()
			.MinimumLevel.Override("Microsoft.EntityFrameworkCore", LogEventLevel.Warning)
			.MinimumLevel.Override("Microsoft.EntityFrameworkCore.Query", dbCommandLogLevel)
			.MinimumLevel.Override("Microsoft.EntityFrameworkCore.Database.Command", dbCommandLogLevel)
			.MinimumLevel.Override("Microsoft.AspNetCore", LogEventLevel.Fatal)
			.MinimumLevel.Override("Microsoft.EntityFrameworkCore.Update", LogEventLevel.Fatal)
			.MinimumLevel.Override("CareCenterServices.Common.Security", LogEventLevel.Warning)
			.MinimumLevel.Override("HealthChecks", LogEventLevel.Warning)
			.Enrich.FromLogContext()
			.Enrich.WithEnvironmentUserName()
			.Enrich.WithMachineName()
			.WriteTo.Console(outputTemplate: OutputTemplate, formatProvider: formatter, theme: AnsiConsoleTheme.Literate, restrictedToMinimumLevel: LogEventLevel.Information)
			.WriteTo.Debug(LogEventLevel.Information, formatProvider: formatter)
			//.WriteTo.RollingFile(new Serilog.Formatting.Compact.CompactJsonFormatter(), "logs//{Date}.log", LogEventLevel.Information)
			.WriteTo.EventLog("Care Center Services", formatProvider: formatter, manageEventSource: true, restrictedToMinimumLevel: LogEventLevel.Information);

		return logConfig.CreateLogger();
	}
}
```