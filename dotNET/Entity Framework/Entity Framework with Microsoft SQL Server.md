```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
	services.AddDbContextPool<CareCenterContext>((serviceProvider, options) =>  
	{  
		options.ConfigureWarnings(builder =>  
		{  
			builder.ConfigureContextWarnings(HostingEnvironment);  
		}); 
	
		var connectionString = serviceProvider
			.GetRequiredService<IConfiguration>()
			.GetConnectionString("MyDbConnection");
	
		options  
			.ReplaceService<IValueConverterSelector, StronglyTypedIdValueConverterSelector>()  
			.UseSqlServer(connectionString, o => o.EnableRetryOnFailure(3));  
	});
    // ...
}
```
