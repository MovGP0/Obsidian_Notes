Attribute
```csharp
[AttributeUsage(AttributeTargets.Method)]  
public sealed class SwaggerResponseContentTypeAttribute : Attribute  
{  
	public SwaggerResponseContentTypeAttribute(string responseType, string responseDescription = "", bool exclusive = true)  
	{  
		ResponseType = responseType;  
		ResponseDescription = responseDescription;  
		Exclusive = exclusive;  
	}  
	  
	/// <summary>  
	/// Response Content Type  
	/// </summary>  
	public string ResponseType { get; }  
	  
	/// <summary>  
	/// Response Content Description  
	/// </summary>  
	public string ResponseDescription { get; }  
	  
	/// <summary>  
	/// Remove all other Response Content Types  
	/// </summary>  
	public bool Exclusive { get; }  
}
```

Operation Filter
```csharp
public sealed class ResponseContentTypeOperationFilter : IOperationFilter  
{  
	public void Apply(OpenApiOperation operation, OperationFilterContext context)  
	{  
		if (!context.ApiDescription.TryGetMethodInfo(out var methodInfo))  
		{  
			return;  
		}  
		  
		var requestAttributes = methodInfo  
			.GetCustomAttributes(true)  
			.OfType<SwaggerResponseContentTypeAttribute>()  
			.FirstOrDefault();  
		  
		if (requestAttributes == null) return;  
		  
		if (requestAttributes.Exclusive)  
		{  
			operation.Responses.Clear();  
		}  
		  
		operation.Responses.Add(requestAttributes.ResponseType, new OpenApiResponse { Description = requestAttributes.ResponseDescription });  
	}  
}
```

Register in `AddSwaggerGen`
```csharp
services.AddSwaggerGen(c =>  
{
	c.OperationFilter<ResponseContentTypeOperationFilter>();
});
```