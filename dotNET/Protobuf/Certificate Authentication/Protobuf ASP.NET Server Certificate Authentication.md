## Configure certificate authentication

Require Client to provide a certificate
```csharp
options.ConfigureHttpsDefaults(opts =>
{
    // ...
	opts.ClientCertificateMode = ClientCertificateMode.RequireCertificate;
});
```

Configure certificate authentication
```csharp
services
    .AddAuthentication(CertificateAuthenticationDefaults.AuthenicationScheme)
    .AddCertificate(opts =>
    {
        opts.AllowedCertificateTypes = CertificateTypes.All;
		// opts.AllowedCertificateTypes = CertificateTypes.Chained;
	    // opts.RevocationMode = X509RevocationMode.NoCheck;
        opts.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = async context =>
            {
		        Claims[] claims = await /*...*/;
		        var identity = new ClaimsIdentity(claims, context.Scheme.Name);
		        context.Principal = new ClaimsPrincipal(identity);
                context.Success();
            },
            OnAuthenticationFailed = async context =>
            {
                // context.ClientCertificate.Tumbprint
                // context.ClientCertificate.Subject
                context.NoResult();
                context.Response.StatusCode = (int)HttpStatusCode.Forbidden;
                context.Response.ContentType = MediaTypeNames.Text.Plain;
                await context.Response.WriteAsync(context.Exception.ToString());
            }
        };
	    // ...
    });
```

Enable Authentication
```csharp
app.UseAuthentication();
```

Check authentication in Protobuf service
```csharp
public override async Task<Person> GetPerson(int id, ServerCallContext context)
{
	if(context.AuthContext.IsPeerAuthenticated)
	{
		// context.AuthContext.PeerIdentityPropertyName
		// foreach(var prop in context.AuthContext.Properties)
	}
}
```

Override certificate validation (in DEV environment)
```csharp
if (env.IsDevelopment())
{
	ServicePointManager.ServerCertificateValidationCallback = new RemoteCertificateValidationCallback(ValidateClientCertificate);
}

private bool ValidateClientCertificate(object sender, X509Certificate certificate, X509Chain chain, SslPolicyErrors sslPolicyErrors)
{
    // return true;
}
```
