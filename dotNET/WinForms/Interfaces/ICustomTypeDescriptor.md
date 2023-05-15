Provides an interface that supplies dynamic custom type information for an object.

## Example

```csharp
public class YourCustomObject : ICustomTypeDescriptor
{
    // Implement the required members of the ICustomTypeDescriptor interface

    public AttributeCollection GetAttributes()
    {
        // Return the custom attributes for your object
        return TypeDescriptor.GetAttributes(this, true);
    }

    public string GetClassName()
    {
        // Return the class name for your object
        return TypeDescriptor.GetClassName(this, true);
    }

    public string GetComponentName()
    {
        // Return the component name for your object
        return TypeDescriptor.GetComponentName(this, true);
    }

    public TypeConverter GetConverter()
    {
        // Return the type converter for your object
        return TypeDescriptor.GetConverter(this, true);
    }

    public EventDescriptor GetDefaultEvent()
    {
        // Return the default event for your object
        return TypeDescriptor.GetDefaultEvent(this, true);
    }

    public PropertyDescriptor GetDefaultProperty()
    {
        // Return the default property for your object
        return TypeDescriptor.GetDefaultProperty(this, true);
    }

    public object GetEditor(Type editorBaseType)
    {
        // Return the editor for the specified base type for your object
        return TypeDescriptor.GetEditor(this, editorBaseType, true);
    }

    public EventDescriptorCollection GetEvents()
    {
        // Return the event descriptors for your object
        return TypeDescriptor.GetEvents(this, true);
    }

    public EventDescriptorCollection GetEvents(Attribute[] attributes)
    {
        // Return the event descriptors for your object filtered by the specified attributes
        return TypeDescriptor.GetEvents(this, attributes, true);
    }

    public PropertyDescriptorCollection GetProperties()
    {
        // Return the property descriptors for your object
        return TypeDescriptor.GetProperties(this, true);
    }

    public PropertyDescriptorCollection GetProperties(Attribute[] attributes)
    {
        // Return the property descriptors for your object filtered by the specified attributes
        return TypeDescriptor.GetProperties(this, attributes, true);
    }

    public object GetPropertyOwner(PropertyDescriptor pd)
    {
        // Return the owner object of the specified property descriptor
        return this;
    }

    // Additional members and properties of your custom object class
}
```

## See also

- [ICustomTypeDescriptor](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.icustomtypedescriptor)