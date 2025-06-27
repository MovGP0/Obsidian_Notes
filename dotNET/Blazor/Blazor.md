## Create Blazor App

Install Blazor Templates
```powershell
dotnet new -i Microsoft.AspNetCore.Blazor.Templates
```

Create Blazor app
```powershell
# Server-Side Blazor
dotnet new blazor -o MyBlazorApp

# Client-Side Blazor
dotnet new blazorwasm -o MyBlazorApp
```

## Create bindable parameters

Child component
```razor
@code
{
	private string _displayText;
	
	[Parameter]
	public string DisplayText
	{
		get => _displayText;
		set
		{
			if (_displayText != value)
			{
				_displayText = value;
				DisplayTextChanged.InvokeAsync(value);
			}
		}
	}
	
	[Parameter] 
	public EventCallback<string> DisplayTextChanged { get; set; }
}
```

Parent component
```razor
@layout BaseLayout
@page "/parent"

<h3>Parent Component</h3>
<p>Child component's DisplayText: @childDisplayText</p>

<ChildComponent 
	DisplayText="@childDisplayText"
	DisplayTextChanged="OnDisplayTextChanged" />

@code
{
	private string childDisplayText = "Hello, World!";
	
	private async Task OnDisplayTextChanged(string newText)
	{
		childDisplayText = newText;
		await Task.CompletedTask;
	}
}
```

## Create Code-Behind file

File `MyComponent.cshtml.cs`
```csharp
using Microsoft.AspNetCore.Blazor.Components;

namespace MyProject;

public abstract class MyComponent : BlazorComponent
{
    public string Title { get; set; } = string.Empty;
}
```

File `MyComponent.cshtml`
```razor
@page /mycomponent
@inherits MyComponent

<h1>@Title</h1>
```


