```csharp
public sealed class EnumSchemaFilter : ISchemaFilter  
{  
	public void Apply(OpenApiSchema schema, SchemaFilterContext context)  
	{  
		if (context.Type.IsEnum)  
		{  
			var array = new OpenApiArray();  
			array.AddRange(Enum.GetNames(context.Type).Select(n => new OpenApiString(n)));  
			schema.Extensions.Add("x-enumNames", array); // NSwag  
			schema.Extensions.Add("x-enum-varnames", array); // Openapi-generator  
		}  
	}  
}
```

Register in `AddSwaggerGen`
```csharp
services.AddSwaggerGen(c =>  
{  
	c.SchemaFilter<EnumSchemaFilter>();  
});
```