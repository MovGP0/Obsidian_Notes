```csharp
public static class LoggerExtensions  
{  
	public static ILogger Here(
		this ILogger logger,
		[CallerMemberName] string memberName = "",
		[CallerFilePath] string sourceFilePath = "",
		[CallerLineNumber] int sourceLineNumber = 0)
	{  
		return logger  
			.ForContext("MemberName", memberName)
			.ForContext("FilePath", sourceFilePath)
			.ForContext("LineNumber", sourceLineNumber);
	}  
}
```

```csharp
using System.Diagnostics;
using System.Runtime.CompilerServices;
using Microsoft.Data.SqlClient;
using Microsoft.EntityFrameworkCore;
using Polly.Bulkhead;
using Polly.CircuitBreaker;

namespace Serilog;

public static class LoggingExtensions
{
    [DebuggerStepThrough]
    public static async Task<T> ExecuteWithLoggingAsync<T>(
        this ILogger logger,
        Func<Task<T>> action,
        [CallerMemberName] string memberName = "",
        [CallerFilePath] string sourceFilePath = "",
        [CallerLineNumber] int sourceLineNumber = 0)
    {
        logger.Here().Verbose($"Getting {typeof(T).Name}");

        var timer = Stopwatch.StartNew();
        try
        {
            return await action().ConfigureAwait(false);
        }
        catch (LoggedException)
        {
            throw;
        }
        catch (BulkheadRejectedException e)
        {
            logger.Here().Warning(e, e.Message);
            throw new LoggedException(e.Message, e);
        }
        catch (BrokenCircuitException e)
        {
            logger.Here().Warning(e, e.Message);
            throw new LoggedException(e.Message, e);
        }
        catch (TaskCanceledException e)
        {
            logger.Here().Warning(e, e.Message);
            throw new LoggedException(e.Message, e);
        }
        catch (DbUpdateException e) when (e.InnerException != null)
        {
            logger.Here().Error(e.InnerException, e.InnerException.Message);
            throw new LoggedException(e.Message, e);
        }
        catch (SqlException e)
        {
            logger.Here().Warning(e, e.Message);
            throw new LoggedException(e.Message, e);
        }
        catch (Exception e)
        {
            // ReSharper disable ExplicitCallerInfoArgument
            logger.Here(memberName, sourceFilePath, sourceLineNumber).Error(e, e.Message);
            // ReSharper restore ExplicitCallerInfoArgument
            throw new LoggedException(e.Message, e);
        }
        finally
        {
            timer.Stop();
            // ReSharper disable ExplicitCallerInfoArgument
            logger.Here(memberName, sourceFilePath, sourceLineNumber).Verbose("Execution took {ms} ms", timer.ElapsedMilliseconds);
            // ReSharper restore ExplicitCallerInfoArgument
        }
    }

    [DebuggerStepThrough]
    public static async Task ExecuteWithLoggingAsync(
        this ILogger logger,
        Func<Task> action,
        [CallerMemberName] string memberName = "",
        [CallerFilePath] string sourceFilePath = "",
        [CallerLineNumber] int sourceLineNumber = 0)
    {
        logger.Here().Verbose("Executing");

        var timer = Stopwatch.StartNew();
        try
        {
            await action().ConfigureAwait(false);
        }
        catch (LoggedException)
        {
            throw;
        }
        catch (Polly.Bulkhead.BulkheadRejectedException e)
        {
            logger.Here().Warning(e, e.Message);
            throw new LoggedException(e.Message, e);
        }
        catch (TaskCanceledException e)
        {
            logger.Here().Warning(e, e.Message);
            throw new LoggedException(e.Message, e);
        }
        catch (DbUpdateException e) when (e.InnerException != null)
        {
            logger.Here().Error(e.InnerException, e.InnerException.Message);
            throw new LoggedException(e.Message, e);
        }
        catch (Exception e)
        {
            // ReSharper disable ExplicitCallerInfoArgument
            logger.Here(memberName, sourceFilePath, sourceLineNumber).Error(e, e.Message);
            // ReSharper restore ExplicitCallerInfoArgument
            throw new LoggedException(e.Message, e);
        }
        finally
        {
            timer.Stop();
            // ReSharper disable ExplicitCallerInfoArgument
            logger.Here(memberName, sourceFilePath, sourceLineNumber).Verbose("Execution took {ms} ms", timer.ElapsedMilliseconds);
            // ReSharper restore ExplicitCallerInfoArgument
        }
    }
}
```