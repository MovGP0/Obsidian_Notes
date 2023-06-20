## Declare Entities

```csharp
public sealed class Person : XPObject
{
    public Person(Session session) : base(session) { }

    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string FirstName { get; set; } = string.Empty;

    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string LastName { get; set; } = string.Empty;

    [Association("Person-Addresses")]
    public XPCollection<Address> Addresses => GetCollection<Address>("Addresses");
}

public sealed class Address : XPObject
{
    public Address(Session session) : base(session) { }

    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string Street { get; set; } = string.Empty;

    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string City { get; set; } = string.Empty;

    [Size(SizeAttribute.DefaultStringMappingFieldSize)]
    public string Country { get; set; } = string.Empty;

    [Association("Person-Addresses")]
    public Person? Person { get; set; }
}
```

## Declare Interface for Repository

```csharp
public interface IPeopleRepository
{
    // Create
    Person CreatePerson(string firstName, string lastName);
    Address AddAddressToPerson(Person person, string street, string city, string country);

    // Read
    Person GetPersonById(int id);
    XPCollection<Address> GetAddressesByPerson(Person person);

    // Update
    void UpdatePerson(int id, string firstName, string lastName);
    void UpdateAddress(int id, string street, string city, string country);

    // Delete
    void DeletePerson(int id);
    void DeleteAddress(int id);
}
```

## Declare Repository

```csharp
public sealed class PeopleRepository : IPeopleRepository
{
    private readonly UnitOfWork _unitOfWork;

    public PeopleRepository(UnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }

    // Create
    public Person CreatePerson(string firstName, string lastName)
    {
        var person = new Person(_unitOfWork)
        {
            FirstName = firstName,
            LastName = lastName
        };
        _unitOfWork.CommitChanges();  // Save changes to the database

        return person;
    }

    public Address AddAddressToPerson(Person person, string street, string city, string country)
    {
        var address = new Address(_unitOfWork)
        {
            Street = street,
            City = city,
            Country = country,
            Person = person  // Set the Person reference
        };
        _unitOfWork.CommitChanges();  // Save changes to the database

        return address;
    }

    // Read
    public Person GetPersonById(int id)
    {
        return _unitOfWork.GetObjectByKey<Person>(id);
    }

    public XPCollection<Address> GetAddressesByPerson(Person person)
    {
        return person.Addresses;
    }

    // Update
    public void UpdatePerson(int id, string firstName, string lastName)
    {
        var person = GetPersonById(id);
        if (person != null)
        {
            person.FirstName = firstName;
            person.LastName = lastName;
            _unitOfWork.CommitChanges();  // Save changes to the database
        }
    }

    public void UpdateAddress(int id, string street, string city, string country)
    {
        var address = _unitOfWork.GetObjectByKey<Address>(id);
        if (address != null)
        {
            address.Street = street;
            address.City = city;
            address.Country = country;
            _unitOfWork.CommitChanges();  // Save changes to the database
        }
    }

    // Delete
    public void DeletePerson(int id)
    {
        var person = GetPersonById(id);
        if (person != null)
        {
            _unitOfWork.Delete(person);
            _unitOfWork.CommitChanges();  // Save changes to the database
        }
    }

    public void DeleteAddress(int id)
    {
        var address = _unitOfWork.GetObjectByKey<Address>(id);
        if (address != null)
        {
            _unitOfWork.Delete(address);
            _unitOfWork.CommitChanges();  // Save changes to the database
        }
    }
}
```

## Add in Dependency Injection

```csharp
public static class DependencyInjection
{
    public IServiceCollection AddPeopleServices(this IServiceCollection services)
    {
        services.AddScoped<IPeopleRepository, PeopleRepository>();
        // Other service registrations...
        return services;
    }
}
```