
Implement Helper Method to log `DevExpress.Xpo.Logger.LogMessage` properties in Serilog context
```csharp
internal static class LogMessageExtensions
{
    public static ILogger BuildLogger(this LogMessage message)
    {
        return Serilog.Log.Logger
            .ForContext(nameof(LogMessage.MessageType), message.MessageType)
            .ForContext(nameof(LogMessage.ThreadId), message.ThreadId)
            .ForContext(nameof(LogMessage.ThreadName), message.ThreadName)
            .ForContext(nameof(LogMessage.Date), message.Date)
            .ForContext(nameof(LogMessage.Duration), message.Duration)
            .ForContext(nameof(LogMessage.ParameterCount), message.ParameterCount);
    }
}
```

Implement `DevExpress.Xpo.Logger.ILogger`
```csharp
public sealed class XpoSerilogLogger : DevExpress.Xpo.Logger.ILogger
{
    private static Serilog.ILogger Logger => Serilog.Log.Logger;

    public void Log(LogMessage message)
    {
        if (!Enabled)
        {
            Interlocked.Increment(ref _lostMessageCount);
            return;
        }

        switch (message.MessageType)
        {
            case LogMessageType.DbCommand:
            case LogMessageType.LoggingEvent:
            case LogMessageType.Statement:
            case LogMessageType.SessionEvent:
            case LogMessageType.Text:
                if (Logger.IsEnabled(LogEventLevel.Verbose))
                {
                    message.BuildLogger().Here().Verbose(message.MessageText);
                }
                break;
            case LogMessageType.Exception:
                if (Logger.IsEnabled(LogEventLevel.Error))
                {
                    message.BuildLogger().Here().Error(message.MessageText);
                }
                break;
            default:
                if (Logger.IsEnabled(LogEventLevel.Debug))
                {
                    message.BuildLogger().Here().Debug(message.MessageText);
                }
                break;
        }

        Interlocked.Increment(ref _count);
    }

    public void Log(LogMessage[] messages)
    {
        foreach (LogMessage msg in messages)
            Log(msg);
    }

    public void ClearLog()
    {
        throw new System.NotSupportedException();
    }

    private int _count;
    public int Count => _count;

    private int _lostMessageCount;
    public int LostMessageCount => _lostMessageCount;

    public bool IsServerActive => true;
    public bool Enabled { get; set; } = true;
    public int Capacity => 0;
}
```

Add Logger to `LogManager`
```csharp
DevExpress.Xpo.Logger.LogManager.SetTransport(new XpoSerilogLogger());
```

## Notes
- DevExpress does not support different LogLevels.