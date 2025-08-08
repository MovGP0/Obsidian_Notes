Create the `JoinableTaskContext` on the UI thread early in the application:
```csharp
public partial class App : Application
{
    public JoinableTaskContext JoinableTaskContext { get; private set; }

    protected override void OnStartup(StartupEventArgs e)
    {
        base.OnStartup(e);
        // Ensure this is the UI thread
        JoinableTaskContext = new JoinableTaskContext();  
    }
}
```

Now you can run `async` methods within sync methods:

```csharp
public void Foo()
{
    var cts = new CancellationTokenFactory(TimeSpan.FromSeconds(5));
    var ct = cts.Token;

    var taskContext = App.Current.JoinableTaskContext;

    // either
    taskContext.Factory.Run(() => BarAsync(ct));

	// or
	var taskFactory = new JoinableTaskFactory(taskContext);
	taskFactory.Run(() => BarAsync(ct));
}

private async Task BarAsync(CancellationToken cancellationToken = default)
{
    // ...
}
```

Before calling a method that requires `STA`, call
```csharp
await JoinableTaskFactory.SwitchToMainThreadAsync();
```
This will switch the task to the main thread.


When other code should be executed while the async code is executing:
```csharp
    var handle = joinableTaskFactory.RunAsync(async () => await LongRunningAsync());

    // run other code here

    // wait until long running task has finished
    handle.Join(); // or 'await handle'
```

> [!warning]
> - Do not use in server-side or console apps where there's no single-threaded context—just await your async methods.
> - Overuse is a sign of overly synchronized architecture.
> - Nesting `JTF.Run()` inside another `JTF.Run()` is discouraged. Choose a single entry point.
