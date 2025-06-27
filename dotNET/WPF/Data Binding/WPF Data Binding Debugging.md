Binding errors will be shown in the debug output.

## Change the trace level

```xml
<MyControl
	xmlns:diag="clr-namespace:System.Diagnostics;assembly=WindowsBase">
	<TextBlock Text="{Binding UserName, diag:PresentationTraceSources.TraceLevel=High}" />
</MyControl>
```

## Validate bindings with validation rules

```csharp
public sealed class DataContextValidationRule : ValidationRule
{
    public Type ExpectedType { get; set; }

    public override ValidationResult Validate(object value, CultureInfo cultureInfo)
    {
        if (value != null && !ExpectedType.IsInstanceOfType(value))
        {
            return new ValidationResult(false, $"DataContext is not of type {ExpectedType.Name}. Found: {value.GetType().Name}");
        }

        return ValidationResult.ValidResult;
    }
}
```

```xml
<TextBox>
    <TextBox.Text>
        <Binding Path="SomeProperty" UpdateSourceTrigger="PropertyChanged">
            <Binding.ValidationRules>
                <local:DataContextValidationRule ExpectedType="{x:Type vm:ExpectedViewModel}" />
            </Binding.ValidationRules>
        </Binding>
    </TextBox.Text>
</TextBox>
```

## Global Debugging Hook for Binding Errors

Register a custom `System.Diagnostics.TraceListener` in `App.xaml.cs` or `Program.cs`
```csharp
PresentationTraceSources.DataBindingSource.Listeners.Add(new ConsoleTraceListener());
PresentationTraceSources.DataBindingSource.Switch.Level = SourceLevels.Warning | SourceLevels.Error;
```
