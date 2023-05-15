The `ISupportInitialize` and `ISupportInitializeNotification` interfaces are typically used to optimize performance when initializing a component.

The `ISupportInitialize` interface includes `BeginInit` and `EndInit` methods, which are called when the initialization of a component begins and ends, respectively.

The `ISupportInitializeNotification` interface extends `ISupportInitialize` by including an `IsInitialized` property

## Example

```csharp
using System;
using System.ComponentModel;

public class MyComponent : ISupportInitialize, ISupportInitializeNotification
{
    private bool _isInitialized;

    public bool IsInitialized
    {
        get { return _isInitialized; }
    }

    public event EventHandler Initialized;

    public void BeginInit()
    {
        // Initialization is beginning. You might disable certain operations here.
    }

    public void EndInit()
    {
        // Initialization is ending. Perform final setup steps here.

        // After the final setup, mark the component as initialized and raise the Initialized event.
        _isInitialized = true;
        OnInitialized(EventArgs.Empty);
    }

    protected virtual void OnInitialized(EventArgs e)
    {
        Initialized?.Invoke(this, e);
    }
}
```