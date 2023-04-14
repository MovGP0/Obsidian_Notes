```csharp
public sealed class PhoneNumberValidator<T> : PropertyValidator<T, string>
{
	public override bool IsValid(ValidationContext<T> context, string value)
	{
		if (string.IsNullOrEmpty(value)) return true;

		var util = PhoneNumbers.PhoneNumberUtil.GetInstance();
		try
		{
			var number = util.Parse(value, "AT");
			return util.IsValidNumber(number);
		}
		catch
		{
			return false;
		}
	}
	
	public override string Name => "PhoneNumberValidator";
}
```

```csharp
public static class RuleBuilderExtensions
{
	public static IRuleBuilderOptions<T, string> PhoneNumber<T>(
		this IRuleBuilder<T, string> ruleBuilder)
	{
		return ruleBuilder.SetValidator(new PhoneNumberValidator<T>());
	}
}
```