```csharp
public static class RuleBuilderExtensions  
{  
	public static IRuleBuilder<T, string> Password<T>(this IRuleBuilder<T, string> ruleBuilder, int minimumLength = 14)  
	{  
		return ruleBuilder
			.NotEmpty().WithMessage(ErrorMessages.PasswordEmpty)
			.MinimumLength(minimumLength).WithMessage(ErrorMessages.PasswordLength)
			.Matches("[A-Z]").WithMessage(ErrorMessages.PasswordUppercaseLetter)
			.Matches("[a-z]").WithMessage(ErrorMessages.PasswordLowercaseLetter)
			.Matches("[0-9]").WithMessage(ErrorMessages.PasswordDigit)
			.Matches("[^a-zA-Z0-9]").WithMessage(ErrorMessages.PasswordSpecialCharacter);
	}
}
```