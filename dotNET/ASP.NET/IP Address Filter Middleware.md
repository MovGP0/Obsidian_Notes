```csharp
public sealed class IPWhitelistConfiguration  
{  
	public IEnumerable<IPAddressRange> AuthorizedIPAddresses { get; init; } = new List<IPAddressRange>();  
}
```

```csharp
using System.Net;  
using Microsoft.AspNetCore.Http;  
using Microsoft.Extensions.Logging;

public sealed class IPAddressFilterMiddleware  
{  
	private RequestDelegate Next { get; }  
	private ILogger<IPAddressFilterMiddleware> Logger { get; }  
	private IPWhitelistConfiguration Options { get; }  
	  
	public IPAddressFilterMiddleware(  
		RequestDelegate next,  
		ILogger<IPAddressFilterMiddleware> logger,  
		IPWhitelistConfiguration options)  
	{  
		Next = next;
		Logger = logger;
		Options = options;
	}  

	public async Task Invoke(HttpContext context)  
	{  
		var remoteIp = context.Connection.RemoteIpAddress;  
		Logger.LogInformation("Request from Remote IP address: {RemoteIp}", remoteIp);  
		  
		if (Options.AuthorizedIPAddresses.Any(range => range.Contains(remoteIp)))  
		{  
			await Next.Invoke(context).ConfigureAwait(false);  
			return;  
		}  

		Logger.LogWarning("Forbidden Request from Remote IP address: {RemoteIp}", remoteIp);  
		context.Response.StatusCode = (int) HttpStatusCode.Forbidden;  
	}  
}
```

```csharp
using Microsoft.AspNetCore.Builder;  
using Microsoft.Extensions.Configuration;  
using Microsoft.Extensions.DependencyInjection;  
using NetTools;

public static class IPAddressFilterMiddlewareExtensions  
{  
	public static IServiceCollection AddIPAddressFilter(
		this IServiceCollection services,
		IConfigurationSection configuration)  
	{  
		var values = configuration  
			.AsEnumerable()  
			.Select(e => e.Value)  
			.Where(v => !string.IsNullOrEmpty(v))  
			.Select(v => IPAddressRange.TryParse(v, out var range) ? range : null)  
			.Where(v => v != null)  
			.Cast<IPAddressRange>()  
			.ToList();  
		  
		var config = new IPWhitelistConfiguration  
		{  
			AuthorizedIPAddresses = values  
		};  
		services.AddSingleton(config);  
		return services;  
	}  
	  
	public static void UseIPAddressFilterMiddleware(this IApplicationBuilder app)  
	{  
		app.UseMiddleware<IPAddressFilterMiddleware>();  
	}  
}
```

In `ConfigureServices`
```csharp
services.AddIPAddressFilter(Configuration.GetSection("IPAddressWhitelistConfiguration"));
```