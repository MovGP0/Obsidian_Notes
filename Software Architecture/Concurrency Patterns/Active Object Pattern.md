This pattern decouples method execution from method invocation in order to simplify synchronized processing. It uses a scheduler to order the execution of method requests.

## Example

Define the Active Object Interface
```csharp
public interface IActiveObject
{
    void DoSomething();
    Task<int> CalculateSomethingAsync(int x, int y);
    // Define other methods as needed
}
```

Implement the Active Object Class. This class encapsulates the concurrency details and provide the necessary synchronization mechanisms.
```csharp
public class ActiveObject : IActiveObject
{
    private readonly BlockingCollection<Action> _queue = new BlockingCollection<Action>();

    public ActiveObject()
    {
        Task.Factory.StartNew(Worker);
    }

    public void DoSomething()
    {
        _queue.Add(() =>
        {
            // Perform the desired action
        });
    }

    public Task<int> CalculateSomethingAsync(int x, int y)
    {
        var completionSource = new TaskCompletionSource<int>();

        _queue.Add(() =>
        {
            try
            {
                int result = CalculateSomething(x, y);
                completionSource.SetResult(result);
            }
            catch (Exception ex)
            {
                completionSource.SetException(ex);
            }
        });

        return completionSource.Task;
    }

    private void Worker()
    {
        foreach (var action in _queue.GetConsumingEnumerable())
        {
            action.Invoke();
        }
    }

    // Helper method
    private int CalculateSomething(int x, int y)
    {
        // Perform the calculation
    }
}
```

Use the Active Object
```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var activeObject = new ActiveObject();

        // Execute methods asynchronously
        activeObject.DoSomething();

        int result = await activeObject.CalculateSomethingAsync(5, 10);
        Console.WriteLine(result);
    }
}
```