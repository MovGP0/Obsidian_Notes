The Guarded Suspension pattern is a design pattern that is used to manage operations that need to be delayed until a particular condition is met. It's often used in multithreaded applications.

The pattern's name comes from the idea that while an operation is waiting for the right conditions to proceed, the operation is "suspended" and is "guarded" by the condition.

## Example

Thread-safe message queue. The `Dequeue` operation is guarded, such that it will suspend if there are no messages in the queue until a message is available.

```csharp
using System;
using System.Collections.Generic;
using System.Threading;

public class GuardedQueue<T>
{
    private readonly Queue<T> queue = new Queue<T>();
    private readonly object lockObject = new object();

    public void Enqueue(T item)
    {
        lock (lockObject)
        {
            queue.Enqueue(item);
            Monitor.Pulse(lockObject); // Signal the waiting Dequeue thread that item is added.
        }
    }

    public T Dequeue()
    {
        lock (lockObject)
        {
            while (queue.Count == 0) // Guard
            {
                Monitor.Wait(lockObject); // Suspend until condition is met.
            }

            return queue.Dequeue();
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        var guardedQueue = new GuardedQueue<string>();

        new Thread(() =>
        {
            for (int i = 0; i < 10; i++)
            {
                guardedQueue.Enqueue("Message " + i);
                Thread.Sleep(1000); // Simulate delay.
            }
        }).Start();

        new Thread(() =>
        {
            for (int i = 0; i < 10; i++)
            {
                var message = guardedQueue.Dequeue();
                Console.WriteLine("Received: " + message);
            }
        }).Start();
    }
}
```