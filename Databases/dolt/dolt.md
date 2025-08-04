## Installation

Using WinGet
```sh
winget install dolt
```

Using Chocolatey
```sh
choco install dolt
```

[Download MSI Installer](https://github.com/dolthub/dolt/releases)

## Setup

Add NuGet packages
```sh
dotnet add package MySqlConnector
dotnet add package Dapper
```

## Start the Server

Start the server (defaults to `localhost:3306`; no auth)
```sh
dolt sql-server
```

Start the server with arguments
```sh
sql-server --host='127.0.0.1' --port='3306' --data-dir='D:\path\containing\the\repositories',
```

Create connection
```csharp
// using the main branch
var connString = "Server=127.0.0.1;Port=3306;User Id=root;Database=your_repo_name;";

// using a specific branch
var connString = "Server=127.0.0.1;Port=3306;User Id=root;Database=repo_name/targetbranch;";

// connect to the database
using IDbConnection db = new MySqlConnection(connString);

// Dapper query
var users = db.Query<User>("SELECT id, name, email FROM users;").AsList();
```
