## Without Inheritance

```csharp
using ReactiveUI;

public sealed partial class MyControl : UserControl, IViewFor<MyViewModel>
{
    public MyControl()
    {
        // ...
    }

    object? IViewFor.ViewModel
    {
        get => ViewModel;
        set
        {
            if (value is MyViewModel viewModel)
            {
                ViewModel = viewModel;
                return;
            }

            ViewModel = null;
        }
    }

    public MyViewModel? ViewModel { get; set; }
}

public sealed class MyViewModel : ReactiveObject
{
    // ...
}
```

## With Inheritance

Parent control:
```csharp
public partial class MyBaseControl : UserControl, IViewFor<MyBaseViewModel>
{
	public object? ViewModel { get; set; }  

	MyBaseViewModel? IViewFor<MyBaseViewModel>.ViewModel  
	{  
	    get => ViewModel as MyBaseViewModel;  
	    set => ViewModel = value;  
	}
}

public class MyBaseViewModel : ReactiveObject
{
    // ...
}
```

Child control:
```csharp
public sealed class MyChildControl : MyBaseControl, IViewFor<MyChildViewModel>
{
     MyChildViewModel? IViewFor<MyChildViewModel>.ViewModel
     {
         get => ViewModel as MyChildViewModel;
         set => ViewModel = value;
     }
}

public sealed MyChildViewModel : MyBaseViewModel
{
    // ...
}
```
