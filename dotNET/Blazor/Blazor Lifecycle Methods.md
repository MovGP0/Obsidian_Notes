```csharp
// Constructor / property initializers

// called when the component is first initialized
// cannot access DOM
protected override Task OnInitializedAsync()
{
    await base.OnInitializedAsync();
}

// called after the component has received parameters from its parent 
// and when the component is being re-rendered
protected override async Task OnParametersSetAsync()
{
	await base.OnParametersSetAsync();
}

// called before each render operation
// returns true by default to re-render
protected override bool ShouldRender()
{
    base.ShouldRender();
}

// called after the component has finished rendering
// can be used for updating the DOM
protected override async Task OnAfterRenderAsync(bool firstRender)
{
	await base.OnAfterRenderAsync(firstRender);
}

public void Dispose()
{
	// ...
}
```
