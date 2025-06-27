## Database-per-Tenant

Set connection string based on tenant
```csharp
builder.Services.AddDbContext<TenantDbContext>((serviceProvider, options) =>
 {
	var tenant = serviceProvider.GetService<ITenantProvider>()?.GetTenant();
	if (tenant is null) throw new TenantNotSetException();
	options.UseSqlite(tenant.ConnectionString);
});
```

## Discriminator Column

Filter entities based on tenant
```csharp
public partial class TenantDbContext : DbContext
{
    private readonly Tenant _tenant;

    public TenantDbContext(
        ITenantProvider tenantProvider,
        DbContextOptions<TenantDbContext> options)
        : base(options)
    {
        _tenant = tenantProvider.GetTenant();
    }

    public DbSet<Book> Books { get; set; } = default!;
    
	protected override void OnModelCreating(ModelBuilder modelBuilder)
	{
		modelBuilder.Entity<Book>(entity =>
		{
			entity.HasQueryFilter(mt => mt.TenantId == _tenant.Id);
		});
	}
}
```
