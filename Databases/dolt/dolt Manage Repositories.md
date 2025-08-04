---
title: Manage Repositories (Storage Files)
---
## Initialize a repository

Create a new repository in the current directory
```sh
dolt init
```

## Clone a Repository

### using the CLI
```sh
dolt clone https://doltremoteapi.dolthub.com/org/repo
```

### using SQL
```sql
CALL DOLT_CLONE('https://doltremoteapi.dolthub.com/org/repo', 'localName');
```

### Delete a Database

Drop the database
```sql
DROP DATABASE dbName;
```

Reverse the drop
```sql
CALL DOLT_UNDROP('dbName');
```

Permanently delete dropped databases
```sql
CALL DOLT_PURGE_DROPPED_DATABASES();
```
> [!Warning]
> This will permanently delete all dropped databases; not just the current one
