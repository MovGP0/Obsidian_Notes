
## Example

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

public class Program
{
    private static ReaderWriterLockSlim _lock = new ReaderWriterLockSlim();
    private static int _sharedResource = 0;

    public static void Main()
    {
        // Create tasks for multiple readers and a single writer
        Task[] tasks = new Task[10];
        for (int i = 0; i < 9; i++)
        {
            tasks[i] = Task.Factory.StartNew(() => Read());
        }
        tasks[9] = Task.Factory.StartNew(() => Write("TaskW"));

        // Wait for all tasks to complete
        Task.WaitAll(tasks);
    }

    private static void Read()
    {
        _lock.EnterReadLock();
        try
        {
            Console.WriteLine("Read start: " + _sharedResource);
            Thread.Sleep(1000); // simulate the time to read
            Console.WriteLine("Read end: " + _sharedResource);
        }
        finally
        {
            _lock.ExitReadLock();
        }
    }

    private static void Write(string name)
    {
        _lock.EnterWriteLock();
        try
        {
            _sharedResource++;
            Console.WriteLine(name + " writes: " + _sharedResource);
            Thread.Sleep(1000); // simulate the time to write
        }
        finally
        {
            _lock.ExitWriteLock();
        }
    }
}
```