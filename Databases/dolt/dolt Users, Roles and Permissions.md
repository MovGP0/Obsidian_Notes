---
title: Users, Roles and Permissions
---

> [!note] 
> dolt uses MySQL syntax for user and permission management

## Account Names

MySQL accounts are specified as `'username'@'host'`.

`host` can be 
- a hostname (e.g., `'localhost'`)
- an IP address (e.g. `127.0.0.1`)
- or a wildcard pattern such as `'%'` to match any host

## Privilege Levels

Privileges apply at distinct scopes:

| Scope        | Description                             |
| ------------ | --------------------------------------- |
| **Global**   | affects the entire server               |
| **Database** | affects all objects in a given database |
| **Table**    | affects all columns of a table          |
| **Column**   | affects specific columns within a table |
| **Routine**  | affects stored procedures and functions |

## Privilege Types

MySQL provides both **static** and **dynamic/administrative** privileges. 
- Static privileges include `SELECT`, `INSERT`, `UPDATE`, `DELETE`, `CREATE`, `ALTER`, `DROP`, 
- Administrative privileges include `SUPER`, `RELOAD`, `PROCESS`, etc.

## Create and manage user

Create a user
```sql
CREATE USER 'alice'@'localhost' IDENTIFIED BY 'strong_password';
```

## Grant Privileges

Grant Privileges with `GRANT`
```sql
GRANT privilege_list
ON priv_level
TO 'username'@'host'
[WITH GRANT OPTION];
```

**Example**
```sql
GRANT ALL PRIVILEGES
ON *.*
TO 'admin'@'%'
WITH GRANT OPTION;
```

## Revoke Privileges

Revoke all Privileges with `REVOKE`
```sql
REVOKE privilege_list
ON priv_level
FROM 'username'@'host';
```

Revoke a specific privilege:
```sql
REVOKE INSERT
ON analytics_db.*
FROM 'report_user'@'10.0.0.3';
```

Revoke all privileges:
```sql
REVOKE ALL PRIVILEGES, GRANT OPTION
FROM 'legacy_user'@'localhost';
```

## View Current Privileges

```sql
SHOW GRANTS FOR 'alice'@'localhost';
```

Query system tables directly:
```sql
SELECT User, Host
FROM mysql.user;
```
```sql
SELECT * 
FROM information_schema.user_privileges 
WHERE GRANTEE LIKE "'alice'@'localhost'";
```

## Roles

A **role** is a named collection of privileges.

### Create a role

```sql
CREATE ROLE 'developer';
GRANT SELECT, INSERT, UPDATE
ON project_db.*
TO 'developer';
```

### Assigning a Role to User

```sql
GRANT 'developer'
TO 'alice'@'localhost';
```

### Default Roles

```sql
SET DEFAULT ROLE 'developer'
TO 'alice'@'localhost';
```

### Change active Role

Set the role within an currently active session:
```sql
SET ROLE 'qa_tester';
```
