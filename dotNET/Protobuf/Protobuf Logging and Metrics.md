Setup Logging
```csharp
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;

public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureLogging(logging =>
        {
            logging.ClearProviders();
            logging.SetMinimumLevel(LogLevel.Information);
            logging.AddConsole(options =>
            {
	            options.FormatterName = ConsoleFormatterNames.Json;
	        });
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

Enable detailed logging
```csharp
services.AddGrpc(options =>
{
   options.EnableDetailedErrors =  env.IsDevelopmentEnvironment();
});
```

Enable Metrics
```csharp
app.UseGrpcMetrics();
```
```csharp
endpoints.MapMetrics();
```

Implement Interceptor
```csharp
using Microsoft.Extensions.Logging;
using Grpc.Core;
using Grpc.Core.Interceptors;
using Prometheus;

namespace SomeNamespace;

public sealed class LoggingInterceptor : Interceptor
{
	private ILogger Log { get; }

    public LoggingInterceptor(ILogger<LoggingInterceptor> log)
    {
	    Log = log;
    }
	
	private static readonly Counter BlockingUnaryCallsCount = 
		Metrics.CreateCounter("blocking_unary_calls_count", "Count of blocking unary calls."); 
	
	private static readonly Counter AsyncUnaryCallsCount = 
		Metrics.CreateCounter("async_unary_calls_count", "Count of async unary calls."); 
		
	private static readonly Counter ClientStreamingCallsCount = 
		Metrics.CreateCounter("client_streaming_calls_count", "Count of client streaming calls."); 

	private static readonly Counter ServerStreamingCallsCount =
		Metrics.CreateCounter("server_streaming_calls_count", "Count of server streaming calls."); 
	
	private static readonly Counter DuplexStreamingCallsCount =
		Metrics.CreateCounter("duplex_streaming_calls_count", "Count of bi-directional streaming calls.");

	private static readonly Counter FailedGrpcCallsCount =
		Metrics.CreateCounter("failed_grpc_calls_count", "Count of failed gRPC calls.");

	private static readonly Histogram GrpcCallDuration =
		Metrics.CreateHistogram("grpc_call_duration", "Duration of gRPC calls.")

    public override TResponse BlockingUnaryCall<TRequest, TResponse>(
	    TRequest request,
	    ClientInterceptorContext<TRequest, TResponse> context,
	    BlockingUnaryCallContinuation<TRequest, TResponse> continuation)
	{
		try
		{
			BlockingUnaryCallsCount.Inc();
			using GrpcCallDuration.NewTimer();
			return continuation(request, context);
		}
		catch(RpcException e)
		{
		    Log.Error(e, "{Message}", e.Message);
		    throw;
		}
	}

	public override AsyncUnaryCall<TResponse> AsyncUnaryCall<TRequest, TResponse>(
		TRequest request,
		ClientInterceptorContext<TRequest, TResponse> context,
	    AsyncUnaryCallContinuation<TRequest, TResponse> continuation
	)
	{
		AsyncUnaryCallsCount.Inc();
		using GrpcCallDuration.NewTimer();
		var call = continuation(request);

		return new AsyncClientStreamingCall<TRequest, TRespose>(
			call.RequestStream,
			HandleCallResponse(call.ResponseAsync),
			call.ResponseHeadersAsync,
			call.GetStatus,
			call.GetTrailers,
			call.Dispose);
	}
	
	public override AsyncClientStreamingCall<TRequest, TResponse>
		AsyncClientStreamingCall<TRequest, TResponse>
	(
		ClientInterceptorContext<TRequest, TResponse> context,
		AsyncClientStreamingCall Continuation<TRequest, TResponse> continuation
	)
	{
		ClientStreamingCallsCount.Inc();
		using GrpcCallDuration.NewTimer();
		var call = continuation(context);
		return new AsyncClientStreamingCall<TRequest, TResponse>(
			call.RequestStream,
			HandleCallResponse(call.ResponseAsync),
			call.ResponseHeadersAsync,
			call.GetStatus,
			call.GetTrailers,
			call.Dispose);
	}

	public override AsyncServerStreamingCall<TResponse>
		AsyncServerStreamingCall<TRequest, TResponse>
	(
		TRequest request,
		ClientInterceptorContext<TRequest, TResponse> context,
		AsyncServer StreamingCallContinuation<TRequest, TResponse> continuation
	)
	{
		try
		{
			ServerStreamingCallsCount.Inc();
			using GrpcCallDuration.NewTimer();
			return continuation(request, context);
		}
		catch (RpcException e)
		{
		    Log.Error(e, "{Message}", e.Message);
			throw;
		}
	}

	public override AsyncDuplexStreamingCall<TRequest, TResponse>
		AsyncDuplexStreamingCall<TRequest, TResponse>
	(
		ClientInterceptorContext<TRequest, TResponse> context,
		AsyncDuplexStreamingCall Continuation<TRequest, TResponse> continuation
	)
	{
		try
		{
			DuplexStreamingCallsCount.Inc();
			using GrpcCallDuration.NewTimer();
			return continuation(context);
		}
		catch (RpcException e)
		{
		    Log.Error(e, "{Message}", e.Message);
			throw;
		}
   }

    private async Task HandleCallResponse<TResponse>(Task<TResponse> call)
    {
	    try
	    {
		    using GrpcCallDuration.NewTimer();
			var response = await call;
		    return response;
	    }
	    catch(RpcException e)
		{
		    Log.Error(e, "{Message}", e.Message);
		    throw;
		}
    }
}
```

Register Interceptor
```csharp
services.AddSingleton<LoggingInterceptor>();
```
