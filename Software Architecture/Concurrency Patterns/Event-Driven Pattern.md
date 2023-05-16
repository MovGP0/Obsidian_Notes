## Examples

## Classical Event Handler

```csharp
using System;

public class Program
{
    public static void Main()
    {
        // Create a new button
        Button button = new Button();

        // Subscribe to the button's Click event
        button.Click += Button_Click;

        // Simulate a button click
        button.OnClick();
    }

    private static void Button_Click(object sender, EventArgs e)
    {
        Console.WriteLine("Button was clicked!");
    }
}

public class Button
{
    // Declare the Click event
    public event EventHandler Click;

    public void OnClick()
    {
        // Raise the Click event
        Click?.Invoke(this, EventArgs.Empty);
    }
}
```

### Using System.Reactive

```csharp
using System;
using System.Windows.Forms;
using System.Reactive.Linq;

public class Program
{
    public static void Main()
    {
        // Create a new form
        Form form = new Form();

        // Create a new button
        Button button = new Button();
        button.Text = "Click me";
        form.Controls.Add(button);

        // Create an observable from the button's Click event
        var clicks = Observable.FromEventPattern(button, "Click");

        // Subscribe to the observable
        using var subscribtion = clicks.Subscribe(evt =>
        {
            Console.WriteLine("Button was clicked!");
        });

        // Run the form
        Application.Run(form);
    }
}
```
