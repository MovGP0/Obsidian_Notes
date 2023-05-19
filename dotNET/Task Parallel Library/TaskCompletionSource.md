## Example

```csharp
using System;
using System.Threading.Tasks;

public class Program
{
    public static async Task Main()
    {
        // Create a new button
        Button button = new Button();

        // Wait for the button to be clicked
        await button.ClickAsync();

        Console.WriteLine("Button was clicked!");
    }
}

public class Button
{
    private TaskCompletionSource<bool> tcs;

    public Button()
    {
        tcs = new TaskCompletionSource<bool>();
    }

    public Task ClickAsync()
    {
        return tcs.Task;
    }

    public void OnClick()
    {
        // Simulate a button click
        tcs.SetResult(true);
    }
}
```