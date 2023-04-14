```csharp
public interface IPerformContext  
{  
	IPerformContext WriteCancelled();  
	IPerformContext WriteSuccess(string message = "Done");  
	IPerformContext WriteError(string message);  
	IPerformContext WriteWarning(string message);  
	IPerformContext WriteInformation(string message);  
	IPerformContext WriteUserInformation();  
	IPerformContext WriteVerbose(string message);  
	IProgressBar WriteProgressBar(int value = 0, ConsoleTextColor? consoleTextColor = null);  
	IProgressBar WriteProgressBar(string name, int value = 0, ConsoleTextColor? consoleTextColor = null);  
}
```

```csharp
public static class PerformContextExtensions
{
	public static PerformContext WriteSuccess(this PerformContext context, string message)
	{
		if (context == null) return null;
		context.SetTextColor(ConsoleTextColor.Green);
		context.WriteLine(message);
		context.ResetTextColor();
		return context;
	}

	public static PerformContext WriteError(this PerformContext context, string message)
	{
		if (context == null) return null;
		context.SetTextColor(ConsoleTextColor.Red);
		context.WriteLine(message);
		context.ResetTextColor();
		return context;
	}

	public static PerformContext WriteWarning(this PerformContext context, string message)
	{
		if (context == null) return null;
		context.SetTextColor(ConsoleTextColor.Yellow);
		context.WriteLine(message);
		context.ResetTextColor();
		return context;
	}

	public static PerformContext WriteInformation(this PerformContext context, string message)
	{
		if (context == null) return null;
		context.SetTextColor(ConsoleTextColor.White);
		context.WriteLine(message);
		context.ResetTextColor();
		return context;
	}

	public static PerformContext WriteVerbose(this PerformContext context, string message)
	{
		if (context == null) return null;
		context.SetTextColor(ConsoleTextColor.Gray);
		context.WriteLine(message);
		context.ResetTextColor();
		return context;
	}
}
```

```csharp
public sealed class LoggingPerformContext : IPerformContext
{
	private static ILogger Logger => Log.ForContext<LoggingPerformContext>();

	public IPerformContext WriteSuccess(string message)
	{
		Logger.Here().Information(message);
		return this;
	}

	public IPerformContext WriteError(string message)
	{
		Logger.Here().Error(message);
		return this;
	}

	public IPerformContext WriteWarning(string message)
	{
		Logger.Here().Warning(message);
		return this;
	}

	public IPerformContext WriteCancelled()
	{
		Logger.Here().Warning("Job cancelled.");
		return this;
	}

	public IPerformContext WriteInformation(string message)
	{
		Logger.Here().Information(message);
		return this;
	}

	public IPerformContext WriteUserInformation()
	{
		var userName = System.Security.Principal.WindowsIdentity.GetCurrent().Name;
		WriteInformation($"Executing as {userName}");
		return this;
	}

	public IPerformContext WriteVerbose(string message)
	{
		Logger.Here().Verbose(message);
		return this;
	}

	public IProgressBar WriteProgressBar(int value = 0, ConsoleTextColor? consoleTextColor = null)
	{
		return new LoggingProgressBar(null, value, consoleTextColor);
	}

	public IProgressBar WriteProgressBar(string name, int value = 0, ConsoleTextColor? consoleTextColor = null)
	{
		return new LoggingProgressBar(name, value, consoleTextColor);
	}
}
```

```csharp
public sealed class LoggingProgressBar : IProgressBar
{
	private static ILogger Logger => Log.ForContext<LoggingProgressBar>();

	public LoggingProgressBar(string name, int value, ConsoleTextColor? color)
	{
		Logger.Here().Verbose($"Creating Progress bar '{name}' with initial value '{value}' and color '{color}'.");
	}

	public void SetValue(int value)
	{
		Logger.Here().Verbose($"new value: {value}");
	}

	public void SetValue(double value)
	{
		Logger.Here().Verbose($"new value: {value}");
	}
}
```

```csharp
public sealed class PerformContextWrapper : IPerformContext
{
	private PerformContext Context { get; }

	public PerformContextWrapper(PerformContext context)
	{
		Context = context ?? throw new ArgumentNullException(nameof(context));
	}

	public IPerformContext WriteSuccess(string message)
	{
		Context.WriteSuccess(message);
		return this;
	}

	public IPerformContext WriteError(string message)
	{
		Context.WriteError(message);
		return this;
	}

	public IPerformContext WriteWarning(string message)
	{
		Context.WriteWarning(message);
		return this;
	}

	public IPerformContext WriteCancelled()
	{
		Context.WriteWarning("Job cancelled.");
		return this;
	}

	public IPerformContext WriteInformation(string message)
	{
		Context.WriteInformation(message);
		return this;
	}

	public IPerformContext WriteUserInformation()
	{
		var userName = System.Security.Principal.WindowsIdentity.GetCurrent().Name;
		WriteInformation($"Executing as {userName}");
		return this;
	}

	public IPerformContext WriteVerbose(string message)
	{
		Context.WriteVerbose(message);
		return this;
	}

	public IProgressBar WriteProgressBar(int value = 0, ConsoleTextColor consoleTextColor = null)
	{
		return Context.WriteProgressBar(value, consoleTextColor);
	}

	public IProgressBar WriteProgressBar(string name, int value = 0, ConsoleTextColor consoleTextColor = null)
	{
		return Context.WriteProgressBar(name, value, consoleTextColor);
	}
}
```