```powershell
dotnet add package Microsoft.AspNetCore.Components.WebAssembly.Authentication
```

Register Callback URL for your app with the authenication provider

Setup Client-ID in `appsettings.json`
```json
{
	"Authentication":
	{
		"Google":
		{
			"ClientId": "your-client-id.apps.googleusercontent.com"
		}
	}
}
```

Register Authentication provider in `Program.cs`
```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Authentication;
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
```
```csharp
builder.Services.AddOidcAuthentication(options =>
{
	builder.Configuration.Bind("Authentication:Google", options.ProviderOptions);
});
```

Setup `MainLayout.razor`
```razor
<CascadingAuthenticationState>
	<RemoteAuthenticatorView />
	<div class="main">
		<div class="top-row px-4">
			<LoginDisplay />
		</div>
		<div class="content px-4">@Body</div>
	</div>
</CascadingAuthenticationState>
```

Implement `LoginDisplay`
```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager
@inject AuthenticationStateProvider AuthStateProvider

<AuthorizeView>
	<Authorized>
		<button class="nav-link btn btn-link" @onclick="BeginSignOut">Log out</button>
	</Authorized>
	<NotAuthorized>
		<button class="nav-link btn btn-link" @onclick="BeginSignIn">Log in</button>
	</NotAuthorized>
</AuthorizeView>

@code
{
	private async Task BeginSignIn()
	{
		await AuthStateProvider.GetAuthenticationStateAsync();
		Navigation.NavigateTo("authentication/login");
	}
	
	private async Task BeginSignOut()
	{
		await SignOutManager.SetSignOutState();
		Navigation.NavigateTo("authentication/logout");
	}
}
```
