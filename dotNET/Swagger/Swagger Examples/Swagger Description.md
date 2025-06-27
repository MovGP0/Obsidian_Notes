Attribute
```csharp
[AttributeUsage(AttributeTargets.Method)]  
public sealed class SwaggerDescription : Attribute  
{  
	public string Text { get; }  
	  
	public SwaggerDescription(string text)  
	{  
		Text = text;  
	}  
}
```

OperationFilter
```csharp
public sealed class DescriptionOperationFilter : IOperationFilter  
{  
	public void Apply(OpenApiOperation operation, OperationFilterContext context)  
	{  
		if (!context.ApiDescription.TryGetMethodInfo(out var methodInfo))  
		{  
			return;  
		}  

		var requestAttributes = methodInfo  
			.GetCustomAttributes(true)  
			.OfType<SwaggerDescription>()  
			.FirstOrDefault();  
		  
		if (requestAttributes == null) return;  
		if (string.IsNullOrWhiteSpace(requestAttributes.Text)) return;  

		operation.Description = requestAttributes.Text;  
	}  
}
```

Register in `AddSwaggerGen`
```csharp
services.AddSwaggerGen(c =>  
{  
	c.SwaggerDoc("v1", new OpenApiInfo { Title = "MyAPI", Version = "v1" });  
	c.MapType<DateTime>(() => new OpenApiSchema { Type = "string", Format = "date" });  
	c.OperationFilter<ResponseContentTypeOperationFilter>();  
	c.OperationFilter<DescriptionOperationFilter>();  
	c.SchemaFilter<EnumSchemaFilter>();  
});
```
