Setup in `Program.cs`

```csharp
var host = Host.CreateDefaultBuilder(args)
	.ConfigureServices((hostContext, services) =>
	{
		services.UseMicrosoftDependencyResolver();
		var resolver = Locator.CurrentMutable;
		resolver.InitializeSplat();
		resolver.InitializeReactiveUI();

		var configuration = hostContext.Configuration;
		services.Setup(configuration);
	})
	.ConfigureAppConfiguration((builderContext, config) =>
	{
		var env = builderContext.HostingEnvironment;

		config
			.SetBasePath(Directory.GetCurrentDirectory())
			.AddJsonFile("appsettings.json", false, true)
			.AddJsonFile($"appsettings.{env.EnvironmentName}.json", true, true)
			.AddEnvironmentVariables()
			.AddCommandLine(args);
	})
	.ConfigureLogging(config =>
	{
		config.AddSerilog();
	});
```

## ViewModel

```csharp
public sealed class MyViewModel : ReactiveObject, IActivatableViewModel
{
	public MyViewModel()
	{
		if (this.IsInDesignMode())
		{
			// TODO: provide properties with default values
			/* EXAMPLES
			SomeProperty = true;
			Customers.Add(new());
			SelectedCustomer = new();
			DoSomething = EnabledCommand.Instance;
			*/
		}

		this.WhenActivated(d =>  
		{
			var behaviours = Locator.Current.GetServices<IBehaviour<MyViewModel>>();  
			foreach (var behaviour in behaviours)  
			{  
				behaviour.Activate(this, d);  
			}
		}
	}

	[Reactive] public bool SomeProperty { get; set; } = false;

	public IObservableCollection<CustomerInfoViewModel> Customers { get; } = new ObservableCollectionExtended<CustomerInfoViewModel>();  
	[Reactive] public CustomerInfoViewModel? SelectedCustomer { get; set; }

	[Reactive] public ReactiveCommand<Unit, Unit> DoSomething { get; set; } = DisabledCommand.Instance;

	public ViewModelActivator Activator { get; } = new();
}
```

### View Model Behaviour

```csharp
public interface IBehaviour<in T>  
	where T : ReactiveObject  
{  
	void Activate(T viewModel, CompositeDisposable compositeDisposable);  
}
```

### Default Commands

```csharp
///<summary>Use this as a default value for commands</summary>
public static class DisabledCommand  
{  
	public static readonly ReactiveCommand<Unit, Unit> Instance  
		= ReactiveCommand.Create<Unit, Unit>(_ => Unit.Default, Observable.Return(false));  
}

///<summary>Use this to set commands when in Design Mode</summary>
public static class EnabledCommand  
{  
	public static readonly ReactiveCommand<Unit, Unit> Instance  
		= ReactiveCommand.Create<Unit, Unit>(_ => Unit.Default, Observable.Return(true));  
}
```

## View

Bind to property
```xml
<TextBlock Margin="4"  
	VerticalAlignment="Center"  
	Width="120"  
	Text="{x:Static local:Resources.Customer}" />
<TextBox Margin="4" 
	 VerticalAlignment="Center"
	 Width="120"
	 Text="{Binding SearchCustomer, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}" />
```

Bind to list of items
```xml
<ListView SelectionMode="Single"  
	ItemsSource="{Binding Path=Customers, UpdateSourceTrigger=PropertyChanged, Mode=OneWay}"  
	SelectedItem="{Binding Path=SelectedCustomer, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}">  
	<ListView.ItemTemplate>  
		<DataTemplate>  
			<adapter:CustomerInfoControl  
				Width="280"  
				DataContext="{Binding}"/>  
		</DataTemplate>  
	</ListView.ItemTemplate>  
</ListView>
```

Bind to Command
```xml
<Button  
	Margin="4"  
	Content="{x:Static local:Resources.SaveChanges}"  
	Command="{Binding SaveChanges, Mode=OneWay, UpdateSourceTrigger=PropertyChanged}" />
```