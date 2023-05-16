Provides functionality to discover the schema for a bindable list, where the properties available for binding differ from the public properties of the object to bind to. `ITypedList` is only used by Visual Studio Designer.

Base interface for `BindingList<T>`, which provides a generic collection that supports data binding.

Implemented by the `System.Data.DataView` and the derived `System.Windows.Forms.BindingSource` class.

## Example

```csharp
public class YourClass : ITypedList
{
    public PropertyDescriptorCollection GetItemProperties(PropertyDescriptor[] listAccessors)
    {
        // Implement the GetItemProperties method to return the collection of properties for your class.
        // This method should return a PropertyDescriptorCollection.

        // If your class has custom properties, you can create a custom PropertyDescriptorCollection and return it.
        // For example:
        // var properties = new List<PropertyDescriptor>
        // {
        //     new CustomPropertyDescriptor(nameof(Property1), typeof(YourClass)),
        //     new CustomPropertyDescriptor(nameof(Property2), typeof(YourClass)),
        //     // Add other custom property descriptors
        // };
        // return new PropertyDescriptorCollection(properties.ToArray());

        // If your class is a collection or list, you can use the TypeDescriptor.GetProperties method to obtain the
        // default PropertyDescriptorCollection for the element type of your collection or list.
        // For example, if your class is a List<YourElementType>, you can return it like this:
        // return TypeDescriptor.GetProperties(typeof(YourElementType));

        // If your class does not have any properties, you can return null.
        return null;
    }

    public string? GetListName(PropertyDescriptor[] listAccessors)
    {
        // Implement the GetListName method to return the name of your class or collection.
        // This method should return a string.

        // If your class represents a collection or list, you can return the name of your class or the name of your
        // collection.
        // For example:
        // return "YourClass";

        // If your class does not represent a collection or list, you can return null or an empty string.
        return null;
    }
}
```

## See also

- [ITypedList](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.itypedlist)
- [BindingList&lt;T&gt;](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.bindinglist-1)