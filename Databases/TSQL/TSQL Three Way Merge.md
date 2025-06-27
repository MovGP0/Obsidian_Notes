```sql
MERGE TargetTable AS T
USING SourceTable AS S
ON T.ID = S.ID

-- Update existing rows in the target table
WHEN MATCHED THEN
    UPDATE SET T.Name = S.Name, T.Age = S.Age

-- Insert new rows from the source table into the target table
WHEN NOT MATCHED BY TARGET THEN
    INSERT (ID, Name, Age)
    VALUES (S.ID, S.Name, S.Age)

-- Delete rows from the target table that do not exist in the source table
WHEN NOT MATCHED BY SOURCE THEN
    DELETE;

-- Optional: Get the number of affected rows
OUTPUT $action, inserted.*, deleted.*;
```