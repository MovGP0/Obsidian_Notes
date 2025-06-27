```powershell
dotnet add package OpenTelemetry.Api
dotnet add package OpenTelemetry.Exporter.Console
dotnet add package OpenTelemetry.Extensions.Hosting
dotnet add package OpenTelemetry.Instrumentation.AspNetCore --prerelease
```

```csharp
using Microsoft.Extensions.DependencyInjection;
using OpenTelemetry;
using OpenTelemetry.Trace;
using OpenTelemetry.Instrumentation.GrpcNetClient;
```

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddOpenTelemetry()
        .WithTracing(builder => builder
        	.AddGrpcClientInstrumentation(opt => { /**/ })
			.AddHttpClientInstrumentation(opt => { /**/ })
			.AddSqlClientInstrumentation(opt => { /**/ })
            .AddAspNetCoreInstrumentation(opt => { /**/ })
            .AddConsoleExporter(opt => { /**/ }))  
        .WithMetrics(builder => builder
            .AddAspNetCoreInstrumentation(opt => { /**/ })
            .AddHttpClientInstrumentation(opt => { /**/ })
            .AddConsoleExporter(opt => { /**/ }));
}
```

## References

- [The OpenTelemetry .NET Client](https://github.com/open-telemetry/opentelemetry-dotnet)