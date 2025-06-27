This pattern reduces the overhead of acquiring a lock by testing the locking criterion (the 'lock hint') before acquiring the lock. It is required to test again after acquiring the lock, since another thread might have mutated it.

Double Checked Locking pattern has potential problems related to the memory model and the way compilers optimize code. In certain cases, it can lead to subtle bugs where multiple instances of the singleton object are created. The problems arise due to out-of-order execution, reordering of instructions, and different memory visibility guarantees provided by different platforms or compilers.

## Example

Using double-checked-locking to create a Singleton instance on first usage.
```csharp
public class DoubleCheckedLockingSingleton
{
    private static volatile DoubleCheckedLockingSingleton instance;
    private static object lockObject = new object();

    private DoubleCheckedLockingSingleton() { }

    public static DoubleCheckedLockingSingleton GetInstance()
    {
        if (instance == null)
        {
            lock (lockObject)
            {
                if (instance == null)
                {
                    instance = new DoubleCheckedLockingSingleton();
                }
            }
        }

        return instance;
    }
}
```

### Alternative: Lazy Initialization

```csharp
public class LazyInitializationSingleton
{
    private static readonly Lazy<LazyInitializationSingleton> lazyInstance =
        new Lazy<LazyInitializationSingleton>(() => new LazyInitializationSingleton());

    private LazyInitializationSingleton() { }

    public static LazyInitializationSingleton GetInstance()
    {
        return lazyInstance.Value;
    }
}
```

### Alternative: Initialization-on-demand

```csharp
public class InitializationOnDemandSingleton
{
    private InitializationOnDemandSingleton() { }

    private static class SingletonHolder
    {
        internal static readonly InitializationOnDemandSingleton instance = new InitializationOnDemandSingleton();
    }

    public static InitializationOnDemandSingleton GetInstance()
    {
        return SingletonHolder.instance;
    }
}
```
