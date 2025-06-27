## `PersistentAttribute`

Specify a table (for classes) or column (for properties) name.

```csharp
[Persistent("MyPersonsTable")]
public class Person : XPObject
{
    // ...
    [Persistent("MyFirstNameColumn")]
    public string FirstName { get; set; }
    // ...
}
```

## `MapInheritanceAttribute`

Specify how inheritance should be mapped in the database. 

```csharp
[MapInheritance(MapInheritanceType.OwnTable)]
public class Employee : Person
{
    // ...
}
```

## `AssociationAttribute`

Creates a relationship between two classes. The `AssociationAttribute` attribute takes a string parameter that serves as the association's name. 

```csharp
[Association("Person-Addresses")]
public XPCollection<Address> Addresses => GetCollection<Address>("Addresses");
```

## `IndexedAttribute`

Creates an index for the specified field in the database.

```csharp
[Indexed(Unique = true)]
public string Email { get; set; }
```

## `SizeAttribute`

Specifies the size of the column in the database.

```csharp
[Size(200)]
public string FirstName { get; set; }
```

## `KeyAttribute`

Indicates that the field is the primary key.

```csharp
[Key(true)]
public int PersonId { get; set; }
```

## `DeferredDeletionAttribute`

Indicate whether or not the object should be marked as deleted without actually being deleted from the database.

```csharp
[DeferredDeletion(false)]
public class Person : XPObject
{
    // ...
}
```

## `NonPersistentAttribute`

Indicates that a property should not be persisted to the database.

```csharp
[NonPersistent]
public string FullName => $"{FirstName} {LastName}";
```
