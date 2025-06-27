SQL Server 2016+

```sql
-- Creating the table with JSON data
CREATE TABLE SampleTable (
    ID INT PRIMARY KEY,
    JSONData NVARCHAR(MAX)
);

-- Inserting JSON data
INSERT INTO SampleTable (ID, JSONData)
VALUES (1, N'{"key": "value", "array": [1, 2, 3]}');

-- Querying JSON data using JSON_VALUE
SELECT ID, JSON_VALUE(JSONData, '$.key') AS KeyValue
FROM SampleTable
WHERE JSON_VALUE(JSONData, '$.key') = 'value';

-- Querying JSON data using JSON_QUERY
SELECT ID, JSON_QUERY(JSONData, '$.array') AS ArrayData
FROM SampleTable;
```
