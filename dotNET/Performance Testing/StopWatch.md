Don't
```csharp
var start = DateTime.UtcNow;
// do something
var end = DateTime.UtcNow;
TimeSpan duration = end - start;
```

Maybe
```csharp
var stopwatch = Stopwatch.StartNew();
// do something
stopwatch.Stop();
TimeSpan duration = stopwatch.Elapsed;
```

Do
```csharp
var startTicks = Stopwatch.GetTimestamp();
// do someting
var endTicks = Stopwatch.GetTimestamp();
double elapsedSeconds = (double)(endTicks - startTicks) / Stopwatch.Frequency;
TimeSpan duration = TimeSpan.FromSeconds(elapsedSeconds);
```
