```csharp
using Microsoft.Extensions.DependencyInjection;
using ReactiveUI;
using Splat;
using System.ComponentModel;
using System.Diagnostics;
using System.Windows;

namespace ReactiveUI;

public static class ReactiveObjectExtensions
{
    public static IDisposable Scoped<T>(this ReactiveObject obj, out T context)
        where T:class
    {
        var scopeFactory = Locator.Current.GetRequiredService<IServiceProvider>();
        var scope = scopeFactory.CreateScope();
        var services = scope.ServiceProvider;
        Debug.Assert(services is not null);
        context = services.GetRequiredService<T>();
        return scope;
    }

    public static bool IsInDesignMode(this ReactiveObject obj)
    {
        return (bool)DesignerProperties.IsInDesignModeProperty.GetMetadata(typeof(DependencyObject))
            .DefaultValue;
    }
}

```