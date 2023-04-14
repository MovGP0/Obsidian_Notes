Add ReactiveUI to the services
```csharp
using ReactiveUI.Blazor;
using Microsoft.Extensions.DependencyInjection;
using Splat;
using Splat.Microsoft.Extensions.DependencyInjection;

public static async Task Main(string[] args)
{ 
	var builder = WebAssemblyHostBuilder.CreateDefault(args);
	builder.RootComponents.Add<App>("#app");
	builder.Services.AddScoped(sp => new HttpClient
	{ 
		BaseAddress = new Uri(builder.HostEnvironment.BaseAddress)
	});
	builder.Services.AddReactiveUI();

    // register Services here...
    // builder.Services.AddSingleton<IMyService, MyService>();

    // use Splat as dependency resolver 
	builder.Services.UseMicrosoftDependencyResolver();
	var resolver = Locator.CurrentMutable;
	resolver.InitializeSplat();
	resolver.InitializeReactiveUI();

    // execute application
    await builder.Build().RunAsync();
}
```

Derive ViewModels from ReactiveObject
```csharp
using ReactiveUI;
using ReactiveUI.Fody;
using System.Reactive;

public class MyViewModel : ReactiveObject,
	IActivatableViewModel,
	IValidatableViewModel
{
    public ViewModelActivator Activator { get; } = new();
    public ValidationContext ValidationContext { get; } = new ValidationContext();
    
    public MyViewModel()
    {
	    this.WhenActivated(disposables =>
	    {
		    var behaviours = Splat.Locator.GetRequiredServices<IBehaviour<MyViewModel>>();
		    foreach(var behaviour in behaviours)
		    {
		        behaviour.Activate(this, disposables);
		    }

            // TODO: implement validation
            // https://www.reactiveui.net/docs/handbook/user-input-validation/
	    });
    }

    [Reactive] public string Text { get; set; } = string.Empty;
    [Reactive] public ReactiveCommand<Unit, Unit> MyCommand { get; set; } = DisabledCommand.Instance;
}
```

Page
```razor
@page "/mycomponent"
@using ReactiveUI
@inject MyViewModel ViewModel
@implements IViewFor<MyViewModel>
@bind-Activator="ViewModel.Activator"

<ReactiveComponentBase Model="@ViewModel">
	<input @bind="ViewModel.Text" placeholder="enter text..." />
	<button @onclick="ViewModel.MyCommand.Execute">Click me!</button>
	<p>@ViewModel.Text</p>
<ReactiveComponentBase>
```
