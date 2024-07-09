## `dynamic` Keyword

The `dynamic` keyword bypasses compile-time type checking.
Casting an object to dynamic will disable IntelliSense and add rutime performance overhead, because the type needs to be checked ad runtime.

## `ExpandoObject` class

[System.Dynamic.ExpandoObject](https://learn.microsoft.com/en-us/dotnet/api/system.dynamic.expandoobject) allows to dynamically add or remove members at runtime. It implements the `IDynamicMetaObjectProvider` interface to query which members it has at runtime. 

Invoking a member of the `ExpandoObject` will add a rutime performance overhead, because the type needs to be checked ad runtime, similar to dynamic objects.

To manage the members of the `ExpandoObject`, the [Microsoft.CSharp.RuntimeBinder.Binder](https://learn.microsoft.com/en-us/dotnet/api/microsoft.csharp.runtimebinder.binder) class is used.

## Example

```csharp
dynamic expando = new System.Dynamic.ExpandoObject();

// Adding properties
expando.Name = "John";
expando.Age = 30;

// Adding a method
expando.Greet = (Action)(() =>
{
    Console.WriteLine($"Hello, my name is {expando.Name} and I am {expando.Age} years old.");
});

// Invoke the method
expando.Greet();

// list all members of the dynamic object
foreach (var kvp in (IDictionary<string, object>)expando)
{
    Console.WriteLine($"{kvp.Key}: {kvp.Value}");
}
```

The `ExpandoObject` manages the members in an internal dictionary. Members can be added or removed using the `BindGetMember` and `BindSetMember` methods.