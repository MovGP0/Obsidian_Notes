```csharp
using Serilog;
using System.Net;
using System.Diagnostics;

Log.Logger = LoggingFactory.CreateLogger(Configuration);
Encoding.RegisterProvider(CodePagesEncodingProvider.Instance);
ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls13;
Activity.DefaultIdFormat = ActivityIdFormat.W3C;
```
