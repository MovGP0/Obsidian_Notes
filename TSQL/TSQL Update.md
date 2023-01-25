```sql
UPDATE Targettable
SET Columnname = t.[foo]
FROM(
   SELECT [foo], [bar]
   FROM Sourcetable
) t
WHERE t.[bar] = Targettable.[baz]
```

```sql
UPDATE t1
SET Columnname = 'foobar'
FROM Tablename t1
JOIN OtherTable t2 ON t1.[Id] = t2.[TablenameId]
```