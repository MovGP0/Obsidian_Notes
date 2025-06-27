This pattern synchronizes method execution to ensure that only one method at a time runs within an object. It also allows an object's methods to cooperatively schedule their execution sequences.

## Example

```csharp
using System;
using System.Threading;

public class MonitorObjectPatternExample
{
    private static object lockObject = new object(); // The monitor object

    public static void Main()
    {
        // Create multiple threads
        for (int i = 1; i <= 5; i++)
        {
            Thread thread = new Thread(DoWork);
            thread.Name = $"Thread {i}";
            thread.Start();
        }

        Console.ReadKey();
    }

    private static void DoWork()
    {
        // Acquire lock on the monitor object
        lock (lockObject)
        {
            // Critical section - shared resource access
            Console.WriteLine($"{Thread.CurrentThread.Name} is accessing the shared resource.");
            Thread.Sleep(1000); // Simulate some work being done
            Console.WriteLine($"{Thread.CurrentThread.Name} has finished accessing the shared resource.");
        }
    }
}
```