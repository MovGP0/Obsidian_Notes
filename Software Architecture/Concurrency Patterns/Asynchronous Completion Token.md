This pattern helps in efficient demonstration of handling of asynchronous IO operations, requests, etc.

## Example

In C#, the [[TaskCompletionSource]] acts as the Asynchronous Completion Token
```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

public class MyAsyncResult
{
    private readonly TaskCompletionSource<string> tcs = new TaskCompletionSource<string>();

    public Task<string> Task => tcs.Task;

    public void SetCompleted(string result)
    {
        tcs.SetResult(result);
    }

    public void SetError(Exception exception)
    {
        tcs.SetException(exception);
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        var asyncResult = new MyAsyncResult();

        // Start an asynchronous operation
        DoSomethingAsync(asyncResult);

        // Wait for the operation to complete asynchronously
        asyncResult.Task.ContinueWith(task =>
        {
            if (task.Exception != null)
            {
                Console.WriteLine($"Error: {task.Exception.InnerException.Message}");
            }
            else
            {
                Console.WriteLine($"Result: {task.Result}");
            }
        });

        // Do other work while the asynchronous operation is in progress
        Console.WriteLine("Doing other work...");

        // Wait for a key press to exit
        Console.ReadKey();
    }

    public static async Task DoSomethingAsync(MyAsyncResult asyncResult)
    {
        try
        {
            // Simulate an asynchronous operation
            await Task.Delay(2000);

            // Set the result when the operation is complete
            asyncResult.SetCompleted("Operation completed successfully.");
        }
        catch (Exception ex)
        {
            // Set the error if an exception occurs
            asyncResult.SetError(ex);
        }
    }
}
```
