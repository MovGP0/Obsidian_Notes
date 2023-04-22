- **Monitoring**: identify slow user scenarios
- **Profiling**: identify hot methods
- **Benchmark**: help to find the best implementation

## Monitoring Options

- [[StopWatch]]
- *OLD* Performance Counters (`System.Diagnostics.PerformanceCounter`)
- *OLD* Event Counters (`System.Diagnostics.Tracing`)
- *NEW* Metrics API (`System.Diagnostics.Metrics`)
- YARP (`Yarp.Telemetry.Consumption`)
- OpenTelemetry (`OpenTelemetry.Metrics`)
- 3rd Party 
	- AppDynamics
	- Application Insights
	- DynaTrace
	- DataDog
	- NewRelic

## Profiling Options

- dotnet CLI
- Visual Studio
- JetBrains Tools

## Benchmark Options

- [BenchmarkDotNet](https://benchmarkdotnet.org/)

## References

- [Networking Telemetry in .NET](https://learn.microsoft.com/en-us/dotnet/fundamentals/networking/networking-telemetry)
- [Collect metrics](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/metrics-collection)

## See also

- [[Performance Testing Pitfalls]]
- [[Performance Statistics]]