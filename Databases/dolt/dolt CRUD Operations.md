---
title: CRUD Operations
---
### Create
### using SQL
```sql
INSERT INTO users (id, name) VALUES (1, 'Alice');
```

C# Example:
```csharp
string insertSql = "INSERT INTO users (name, email) VALUES (@Name, @Email);";
int rows = db.Execute(insertSql, new { Name = "Alice", Email = "alice@example.com" });
```

### using the CLI
```sh
dolt sql -q "INSERT INTO users (id, name) VALUES (1, 'Alice');"
```

### Read

### using SQL
```sql
SELECT * FROM users;
```

```csharp
var allUsers = db.Query<User>("SELECT * FROM users;").ToList();
```

### using the CLI
```sh
dolt sql -q "SELECT * FROM users;"
```

## Update
### using SQL
```sql
UPDATE users SET email = @Email WHERE id = 42;
```

C# Example:
```csharp
string updateSql = "UPDATE users SET email = @Email WHERE id = @Id;";
db.Execute(updateSql, new { Email = "someone@example.com", Id = 1 });
```

### using the CLI
```sh
dolt sql -q "UPDATE users SET name = 'Bob' WHERE id = 1;"
```

### Delete
### using SQL
```sql
DELETE FROM users WHERE id = 42;
```

C# Example:
```csharp
db.Execute("DELETE FROM users WHERE id = @Id;", new { Id = 1 });
```

### using the CLI
```sh
dolt sql -q "DELETE FROM users WHERE id = 1;"
```

## Stage / Commit

After running your SQL commands, the changes must be staged and committed.
### using SQL
```sql
-- Stage and commit modifications:
CALL DOLT_ADD('-A');
CALL DOLT_COMMIT('-m', 'Updated schema and data');
```

### using the CLI
```sh
dolt add -A
dolt commit -m "Added Alice, updated Bob, removed user Eve"
```
