Implement Entities
```csharp
public sealed class Person : XPObject
{
    public Person(Session session) : base(session) { }

    private string _firstName = string.Empty;
    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string FirstName
    {
        get { return _firstName; }
        set { SetPropertyValue(nameof(FirstName), ref _firstName, value); }
    }

    private string _lastName = string.Empty;
    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string LastName
    {
        get { return _lastName; }
        set { SetPropertyValue(nameof(LastName), ref _lastName, value); }
    }

    [Association("Person-Addresses")]
    public XPCollection<Address> Addresses => GetCollection<Address>("Addresses");
}

public sealed class Address : XPObject
{
    public Address(Session session) : base(session) { }

    private string _street = string.Empty;
    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string Street
    {
        get { return _street; }
        set { SetPropertyValue(nameof(Street), ref _street, value); }
    }

    private string _city = string.Empty;
    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string City
    {
        get { return _city; }
        set { SetPropertyValue(nameof(City), ref _city, value); }
    }

    private string _country = string.Empty;
    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string Country
    {
        get { return _country; }
        set { SetPropertyValue(nameof(Country), ref _country, value); }
    }

    private Person? _person;
    [Association("Person-Addresses")]
    public Person? Person
    {
        get { return _person; }
        set { SetPropertyValue(nameof(Person), ref _person, value); }
    }
}
```
