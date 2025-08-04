---
title:
  - Binary Large Object (BLOB) Data
---
## Create Table

```sql
CREATE TABLE images
(
  id int NOT NULL AUTO_INCREMENT,
  name varchar NOT NULL, -- or varchar(255)
  data longblob NOT NULL,
  PRIMARY KEY (id)
);
```

> [!Note]
> `TINYBLOB` supports up to 255 bytes
> `LONGBLOB` supports up to 4 GB

## Insert Data

### Using SQL

Using SQL to import a file:
```sql
INSERT INTO images (id, name, data)
VALUES (1, 'image.png', LOAD_FILE('/path/to/image.png'));
```

> [!Note]
> Remember to commit changes

Using Dapper:
```csharp
public sealed class FileRecord
{
    public int Id { get; set; }
    public string Name { get; set; }
    public byte[] Data { get; set; }
}

byte[] fileBytes = File.ReadAllBytes("/path/to/image.png");

string sql = "INSERT INTO Files (Name, Data) VALUES (@Name, @Data);";
int rowsAffected = connection.Execute(sql, new { Name = 'image.png', Data = fileBytes });
```

## Read Data

Using Dapper:
```csharp
string sql = "SELECT Data FROM Files WHERE Id = @Id;";
byte[] fileBytes = connection.QueryFirstOrDefault<byte[]>(sql, new { Id = recordId });
```
