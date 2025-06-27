```csharp
public sealed class CustomDateFormatter : IFormatProvider
{
	private IFormatProvider BasedOn { get; }
	private string ShortDatePattern { get; }

	public CustomDateFormatter(string shortDatePattern, IFormatProvider basedOn)
	{
		ShortDatePattern = shortDatePattern;
		BasedOn = basedOn;
	}

	public object GetFormat(Type formatType)
	{
		if (formatType == typeof(DateTimeFormatInfo))
		{
			var basedOnFormatInfo = (DateTimeFormatInfo)BasedOn.GetFormat(formatType);
			var dateFormatInfo = (DateTimeFormatInfo)basedOnFormatInfo.Clone();
			dateFormatInfo.ShortDatePattern = ShortDatePattern;
			return dateFormatInfo;
		}
		return BasedOn.GetFormat(formatType);
	}
}
```

