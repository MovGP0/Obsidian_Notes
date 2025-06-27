
Declare Interface for Repositories
```csharp
public interface IPersonRepository
{
    // Create
    Person CreatePerson(string firstName, string lastName);

    // Read
    Person GetPersonById(int id);

    // Update
    void UpdatePerson(int id, string firstName, string lastName);

    // Delete
    void DeletePerson(int id);
}

public interface IAddressRepository
{
    // Create
    Address AddAddressToPerson(Person person, string street, string city, string country);

    // Read
    XPCollection<Address> GetAddressesByPerson(Person person);

    // Update
    void UpdateAddress(int id, string street, string city, string country);

    // Delete
    void DeleteAddress(int id);
}
```

Implement Repositories
```csharp
public sealed class PersonRepository : IPersonRepository
{
    private readonly UnitOfWork _unitOfWork;

    public PersonRepository(UnitOfWork unitOfWork)
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

    // Read
    public Person GetPersonById(int id)
    {
        return _unitOfWork.GetObjectByKey<Person>(id);
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
}

public sealed class AddressRepository : IAddressRepository
{
    private readonly UnitOfWork _unitOfWork;

    public AddressRepository(UnitOfWork unitOfWork)
    {
        _unitOfWork = unitOfWork;
    }

    // Create
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
    public XPCollection<Address> GetAddressesByPerson(Person person)
    {
        return person.Addresses;
    }

    // Update
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

Register Services in Dependency Injection
```csharp
public static class DependencyInjection
{
    public IServiceCollection AddPeopleServices(this IServiceCollection services)
    {
        services.AddScoped<IPersonRepository, PersonRepository>();
        services.AddScoped<IAddressRepository, AddressRepository>();
        // Other service registrations...
        return services;
    }
}
```