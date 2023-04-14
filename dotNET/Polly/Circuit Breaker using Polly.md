```csharp
builder.Services.AddHttpClient("ClientName", client =>
{
	client.BaseAddress = new Uri("https://api.service.name/v2.5/");
});
```

```csharp
public sealed class WeatherClient : IWeatherClient
{
    private IOptions<WeatherClientOptions> _options;
	private readonly IHttpClientFactory _httpClientFactory;
	private static readonly IMemoryCache _weatherCache = new MemoryCache(new MemoryCacheOptions());

	private static readonly AsyncCircuitBreakerPolicy<HttpResponseMessage> _circuitBreaker = 
		Policy<HttpResponseMessage>
			.Handle<HttpRequestException>()
			.OrTransientHttpError()
			.CircuitBreakerAsync(3, TimeSpan.FromSeconds(10));

	public WeatherClient(
		IHttpClientFactory httpClientFactory,
		IOptions<WeatherClientOptions> options)
	{
	    _httpClientFactory = httpClientFactory;
	    _options = options;
	}

	public async Task<WeatherResponse?> GetCurrentWeather(string city)
	{
		if (_circuitBreaker.CircuitState is CircuitState.Open or CircuitState.Isolated)
		{
		    return _weatherCache.Get<WeatherResponse>(city);
		}

	    var apiKey = _options.Value.ApiKey;
	    var client = _httpClientFactory.CreateClient("ClientName");
	    var response = await _circuitBreaker.ExecuteAsync(() => client.GetAsync($"weather&city={city}?apiKey={apikey}"));
	    var weather = await response.Content.ReadFromJsonAsync<WeatherResponse>();
	    _weatherCache.Set(city, weather);
	    return weather;
	}
}
```

```csharp
app.MapGet("/foobar", async (context, IWeatherClient client) =>
{
    var city = context.Request.Query["city"];
	if (string.IsNullOrWhiteSpace(city))
	{
		context.Response.StatusCode = 540;
		context.Response.ContentType = "text/pain";
		await context.Response.WriteAsync("query parameter 'city' was missing");
		return;
	}

     var response = client.GetCurrentWeather(city);
     var jsonString = JsonSerializer.Serialize(response);

     context.Response.ContentType = "application/json";
     await context.Response.WriteAsync(jsonString);
});
```

## References

- [Polly](https://github.com/App-vNext/Polly)
