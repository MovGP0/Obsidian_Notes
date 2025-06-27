Install `Autofac.Extensions.DependencyInjection` (also installs `Autofac` and `Microsoft.Extensions.DependencyInjection.Abstractions`)

Configure Services
```csharp
public class Startup
{
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        // Register services in IServiceCollection
        services.AddMvc();
        services.AddTransient<IMyService, MyService>();
        services.AddScoped<IMyService, MyService>();
        services.AddSingleton<IMyService, MyService>();

        // Create the container builder.
        var builder = new ContainerBuilder();

        // Populate the services from IServiceCollection.
        builder.Populate(services);

        // Register additional services within Autofac
        builder.RegisterType<MyService>().As<IMyService>(); // Transient
        builder.RegisterInstance<MyService>().As<IMyService>(); // Singleton
        builder.RegisterInstance<MyService>().As<IMyService>().InstancePerLifetimeScope(); // Scoped

        // Build the Autofac container.
        var container = builder.Build();

        // Return the IServiceProvider implementation.
        return new AutofacServiceProvider(container);
    }

    public void Configure(IApplicationBuilder app)
    {
        app.UseMvc();
    }
}
```

## See also

- [Autofac](https://autofac.org/)
- [IoC Performance Comparison](https://github.com/danielpalme/IocPerformance)