```sql
UPDATE Targettable
SET Columnname = t.[foo]
FROM(
   SELECT [foo], [bar]
   FROM Sourcetable
) t
WHERE t.[bar] = Targettable.[baz]
```