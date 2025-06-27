
## Default Middleware

Add Services to application builder
```csharp
builder.Services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme).AddCookie();
builder.Services.AddSession();
builder.Services.AddCors();
builder.Services.AddResponseCompression();
// ...
```

Build Middleware Pipeline
```csharp
var app = builder.Build();
app.UseLogging();
app.UseHSTS();
app.UseHttpsRedirection();
app.UseResponseCompression();
app.UseCors();
app.UseStaticFiles();
app.UseSession();
app.UseRouting();
app.UseAuthentication();
app.MapControllers();
app.MapControllerRoute();

app.UseCustomMiddleware();
```

## References

- [Middleware in ASP.NET Core](https://www.dotnetcurry.com/aspnet-core/middleware-in-aspnetcore)