### `lock` statement (`Monitor`)

- Simplest way to lock a resource.
- Works on reference types.
- Internally, `lock` uses the `Monitor` class, which provides additional methods like `Monitor.Enter`, `Monitor.Exit`, and `Monitor.TryEnter` for finer control.

**Example**

```csharp
private readonly object _lockObj = new object();

public void Method()
{
    lock (_lockObj)
    {
        // Critical section
    }
}
```

### `Mutex`

- Used for inter-process synchronization, meaning it can be used to synchronize threads across different processes.
- More heavyweight compared to other options.

**Example**

```csharp
using (var mutex = new Mutex(false, "Global\\MyApp"))
{
    mutex.WaitOne();
    try
    {
        // Critical section
    }
    finally
    {
        mutex.ReleaseMutex();
    }
}
```

### `Semaphore` and `SemaphoreSlim`
- Semaphore allows a limited number of threads to enter a critical section simultaneously.
- `SemaphoreSlim` is a lighter version of `Semaphore` that is used within a single process and provides better performance.

**Example (`SemaphoreSlim`)**
 
```csharp
private SemaphoreSlim _semaphoreSlim = new SemaphoreSlim(3); // Limit to 3 threads

public async Task DoWorkAsync()
{
    await _semaphoreSlim.WaitAsync();
    try
    {
        // Critical section
    }
    finally
    {
        _semaphoreSlim.Release();
    }
}
```

### `ReaderWriterLockSlim`

- Useful when multiple threads read data and only occasionally write.
- Allows multiple readers or a single writer at a time.

**Example**

```csharp
private ReaderWriterLockSlim _lock = new ReaderWriterLockSlim();

public void ReadData()
{
    _lock.EnterReadLock();
    try
    {
        // Read operation
    }
    finally
    {
        _lock.ExitReadLock();
    }
}

public void WriteData()
{
    _lock.EnterWriteLock();
    try
    {
        // Write operation
    }
    finally
    {
        _lock.ExitWriteLock();
    }
}
```

### `SpinLock`

- A lightweight lock that spins in a busy loop until the resource becomes available.
- Not typically used due to high CPU usage but can be useful in specific low-latency scenarios.

**Example**
```csharp
private SpinLock _spinLock = new SpinLock();

public void DoWork()
{
    bool lockTaken = false;
    try
    {
        _spinLock.Enter(ref lockTaken);
        // Critical section
    }
    finally
    {
        if (lockTaken)
        {
            _spinLock.Exit();
        }
    }
}
```

### `ManualResetEvent` and `AutoResetEvent`

- Thread signaling mechanisms.
- `ManualResetEvent` remains signaled until manually reset, while `AutoResetEvent` automatically resets after a waiting thread is released.

**Example (`AutoResetEvent`)**

```csharp
private AutoResetEvent _event = new AutoResetEvent(false);

public void Thread1()
{
    // Wait for signal
    _event.WaitOne();
}

public void Thread2()
{
    // Signal Thread1
    _event.Set();
}
```

### `Barrier`

- Used to coordinate multiple threads working in phases.
- Ensures that threads reach a specific point before moving on.
  
  **Example**
  
```csharp
var barrier = new Barrier(3); // 3 threads

public void ThreadWork()
{
    // Perform work

    barrier.SignalAndWait(); // Wait for other threads
}
```

### `CountdownEvent`

- Similar to `Barrier`, but counts down from a specified value.
- When the count reaches zero, all waiting threads are released.

**Example**

```csharp
private CountdownEvent _countdown = new CountdownEvent(3); // Wait for 3 threads

public void ThreadWork()
{
    // Perform work

    _countdown.Signal(); // Signal that a thread has completed
    _countdown.Wait();   // Wait for all threads to complete
}
```

### `TaskCompletionSource<T>`

- Allows you to manually control the completion of a task.
- Useful in scenarios where you need to signal the completion of an operation from one thread to another.

**Example**

```csharp
private TaskCompletionSource<bool> _tcs = new TaskCompletionSource<bool>();

public Task WaitAsync()
{
    return _tcs.Task;
}

public void CompleteTask()
{
    _tcs.SetResult(true); // Signal completion
}
```

### `Interlocked` class

- Provides atomic operations for simple data types (like integers) and reference types.
- Useful for simple operations like incrementing/decrementing a value across threads.

**Example**

```csharp
private int _count = 0;

public void Increment()
{
    var newCount = Interlocked.Increment(ref _count);
}
```

### `AsyncLock` (Third-Party Libraries)
- Asynchronous locks for async methods. One popular library is `AsyncEx` that provides `AsyncLock` for managing asynchronous thread synchronization.

**Example**

```csharp
private readonly AsyncLock _mutex = new AsyncLock();

public async Task DoWorkAsync()
{
    using (await _mutex.LockAsync())
    {
        // Critical section
    }
}
```

### `Concurrent Collections`

- Collections like `ConcurrentDictionary`, `ConcurrentQueue`, and `ConcurrentBag` are designed for thread-safe operations without needing explicit locks.

**Example**

```csharp
var concurrentDictionary = new ConcurrentDictionary<int, string>();

concurrentDictionary.TryAdd(1, "Value");
```

These alternatives can be used based on the type of synchronization you need (coarse vs. fine-grained, blocking vs. non-blocking) and the specific use case scenario (CPU-intensive operations, async processing, etc.).