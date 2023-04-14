```csharp
using NodaTime;  
using TimeZoneConverter;

public static class DateTimeZoneExtensions  
{  
	public static TimeZoneInfo ToTimeZoneInfo(this DateTimeZone dateTimeZone)  
	{  
		var windowsTimeZoneName = TZConvert.IanaToWindows(dateTimeZone.Id);  
		return TimeZoneInfo.FindSystemTimeZoneById(windowsTimeZoneName);  
	}  
}
```