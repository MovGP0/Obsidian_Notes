Use `ThreadLocal<T>` class to create thread-local data.

```csharp
using System;
using System.Threading;

class Program
{
    // Declare a ThreadLocal of type int.
    private static ThreadLocal<int> threadLocalData = new ThreadLocal<int>(() => 0);

    static void Main(string[] args)
    {
        // Launch three threads, each of which will increment the thread-local variable.
        new Thread(() =>
        {
            for (int i = 0; i < 10; i++)
            {
                threadLocalData.Value++;
                Console.WriteLine($"Thread A: {threadLocalData.Value}");
                Thread.Sleep(100);
            }
        }).Start();

        new Thread(() =>
        {
            for (int i = 0; i < 10; i++)
            {
                threadLocalData.Value++;
                Console.WriteLine($"Thread B: {threadLocalData.Value}");
                Thread.Sleep(100);
            }
        }).Start();

        new Thread(() =>
        {
            for (int i = 0; i < 10; i++)
            {
                threadLocalData.Value++;
                Console.WriteLine($"Thread C: {threadLocalData.Value}");
                Thread.Sleep(100);
            }
        }).Start();
    }
}
```