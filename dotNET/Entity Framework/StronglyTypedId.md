Implement `ValueConverterSelector`
```csharp
public sealed class StronglyTypedIdValueConverterSelector : ValueConverterSelector  
{  
	private readonly ConcurrentDictionary<(Type ModelClrType, Type ProviderClrType), ValueConverterInfo> _converters = new();  
	  
	public StronglyTypedIdValueConverterSelector(ValueConverterSelectorDependencies dependencies)
		: base(dependencies)  
	{
	}  

	public override IEnumerable<ValueConverterInfo> Select(Type modelClrType, Type? providerClrType = null)  
	{  
		var baseConverters = base.Select(modelClrType, providerClrType);  
		foreach (var converter in baseConverters)  
		{  
			yield return converter;  
		}  
		  
		// Extract the "real" type T from Nullable<T> if required  
		var underlyingModelType = UnwrapNullableType(modelClrType);  
		var underlyingProviderType = UnwrapNullableType(providerClrType);  
		  
		// 'null' means 'get any value converters for the modelClrType'  
		if (underlyingProviderType is not null && underlyingProviderType != typeof(Guid)) yield break;  
		  
		// Try and get a nested class with the expected name.  
		var converterType = underlyingModelType?.GetNestedType(nameof(StronglyTypedIdConverter.EfCoreValueConverter));  
		  
		if (underlyingModelType == null) yield break;  
		if (converterType == null) yield break;  
		  
		var baseType = converterType!.BaseType!.GenericTypeArguments[1]!;  
		yield return _converters.GetOrAdd((underlyingModelType, baseType), _ =>  
		{  
			// Create an instance of the converter whenever it's requested.  
			ValueConverter factory(ValueConverterInfo info) => (ValueConverter)Activator.CreateInstance(converterType, info.MappingHints)!;  
			  
			// Build the info for our strongly-typed ID => Guid converter  
			return new ValueConverterInfo(modelClrType, baseType, factory);  
		});  
	}  

	private static Type? UnwrapNullableType(Type? type)  
	{  
		if (type is null) { return null; }  
			return Nullable.GetUnderlyingType(type) ?? type;  
	}  
}
```