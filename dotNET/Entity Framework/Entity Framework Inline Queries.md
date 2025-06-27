Inline Queries
```csharp
FormattableString query = $"""
SELECT TOP(1) p.* FROM Person p WHERE p.Id = {personId}
""";

var person = await myContext.Database
	.SqlQuery<Person>(query)
	.FirstOrDefaultAsync(cancellationToken)
	.ConfigureAwait(false);
```

[Dapper](https://dapper-tutorial.net/dapper) version
```csharp
const string query = """
SELECT TOP(1) p.* FROM Person p WHERE p.Id = @personId
""";

var parameters = new DynamicParameters();
dynamicParameters.Add("@personId", personId, dbType: Dbtype.Int64, direction:ParameterDirection.Input);

var person = await myContext.Connection
	.QuerySingleOrDefault<Person>(
		query,
		parameters,
		commandType: CommandType.Text);
```
