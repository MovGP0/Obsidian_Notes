```csharp
public sealed class MyContext : DbContext, IMyDbContext
{
	public IDbConnection GetDbConnection() => Database.GetDbConnection();

	public MyContext(DbContextOptions<MyContext> options) : base(options)  
	{  
	}

	public DbSet<Customer> Customers { get; set; } = null!;
	// ...

	protected override void OnModelCreating(ModelBuilder modelBuilder)  
	{
		modelBuilder.SetupCustomer();
		// ...
	}
}
```

```csharp
// ReSharper disable once InconsistentNaming
public sealed class Customer_ModelBuilderExtensions
{
	public static ModelBuilder SetupCustomer(this ModelBuilder modelBuilder)
	{
		return modelBuilder.Entity<Customer>(e => {
			e.ToTable("Customer", "dbo");
			e.HasKey(e => e.Id);

            // TODO: configure properties
            // TODO: configure foreign-keys
		});
	}
}
```

## Configure Properties

```csharp
e.Property(e => e.FirstName)
	.HasColumnName("FirstName");
	.IsRequired()  
	.HasMaxLength(50)  
	.HasColumnType("NVARCHAR(50)")
	.IsUnicode(false);
```
```csharp
e.Property(e => e.Price)
	.HasColumName("Price")
	.HasColumnType("DECIMAL")
	.HasPrecision(25, 8);
```

## Configure Foreign-Keys

In `Order_ModelBuilderExtensions`
```csharp
e.HasMany(e => e.OrderItems).WithOne(e => e.Order).HasForeignKey(e => e.OrderId);
```
