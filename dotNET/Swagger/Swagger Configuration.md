In `ConfigureServices`
```csharp
services.AddSwaggerGen(c =>  
{  
	c.SwaggerDoc("v1", new OpenApiInfo { Title = "CareCenterServices", Version = "v1" });  
	c.MapType<DateTime>(() => new OpenApiSchema { Type = "string", Format = "date" });  
	c.OperationFilter<ResponseContentTypeOperationFilter>();  
	c.OperationFilter<DescriptionOperationFilter>();  
	c.SchemaFilter<EnumSchemaFilter>();  
});
```

In `Configure`
```csharp
app.UseSwagger();

app.UseSwaggerUI(c =>  
{  
	c.SwaggerEndpoint("/swagger/v1/swagger.json", "CareCenterServices V1");  
});
```

## Document ASP.NET Controller

```csharp
[SwaggerDescription("Gets a customer by ID")]  
[HttpGet("customer/{id}")]  
[ProducesResponseType(typeof(Customer), (int)HttpStatusCode.OK)]  
[ProducesResponseType(typeof(string), (int)HttpStatusCode.InternalServerError)]  
[ProducesResponseType(typeof(Dictionary<string, string[]>), (int)HttpStatusCode.BadRequest)]  
public IActionResult Get([FromPath]long id)
{
	// ...
}
```

## See also

- [[Swagger Description]]
- [[Swagger Response Content Type]]
- [[Swagger Enums]]