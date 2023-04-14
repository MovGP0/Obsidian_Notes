```csharp
public override async Task<int> SaveChangesAsync(CancellationToken cancellationToken = new())  
{  
	await Customers
		.AuditEntityChanges<Customer, long>(e => e.Id, cancellationToken)
		.ConfigureAwait(false);  
  
	return await base  
		.SaveChangesAsync(cancellationToken)  
		.ConfigureAwait(false);  
}
```

```csharp
private static async Task AuditEntityChanges<T, TId>(
	this IDbSet<T> dbSet,
	Func<T, TId> getId, CancellationToken cancellationToken)  
	where T : class  
{  
	var changes = ChangeTracker.Entries<T>().ToArray();  
	if (changes.Length == 0) return;  
	
	var connectionString = new SqlConnectionStringBuilder(GetDbConnection().ConnectionString);  

	var mapping = Model.FindEntityType(typeof(T));  
	if (mapping is null) return;

	var tableName = Klienten.EntityType.GetTableName();  
	if (tableName is null) return;  
  
	var sqlSchema = StoreObjectIdentifier.Table(tableName, dbSet.EntityType.GetSchema());  
	
	await AuditRows.AddAsync(@ar, cancellationToken).ConfigureAwait(false);  
	  
		switch (change.State)  
		{  
			case EntityState.Modified:  
			{  
				foreach (var prop in change.Properties.Where(p => p.IsModified))  
				{  
					var sqlColumnName = mapping.FindProperty(prop.Metadata.Name)?.GetColumnName(soi);  
					var origValue = prop.OriginalValue?.ToString() ?? string.Empty,
					var newValue = prop.CurrentValue?.ToString() ?? string.Empty
					// ...
				}

				break;
			}

			case EntityState.Added:
			{  
				foreach (var prop in change.Properties)
				{
					var sqlColumnName = mapping.FindProperty(prop.Metadata.Name)?.GetColumnName(soi);
					var origValue = prop.OriginalValue?.ToString() ?? string.Empty,
					var newValue = prop.CurrentValue?.ToString() ?? string.Empty
				};  
		        // ...
				break;  
			}

            case EntityState.Deleted: 
	        {
		        // ...
				break;  
	        }
		}  
	}  
}
```