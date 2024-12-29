**Cypher** is Neo4j’s graph query language, designed to read and write data in graph databases in a pattern-oriented and declarative way.

## Nodes, Labels, Properties, and Relationships

- **Nodes** represent entities or objects (e.g., people, products, or locations).
- **Labels** categorize nodes into different classes (like a “Person” or “Movie”).
- **Properties** store node or relationship attributes (e.g., `name`, `age`).
- **Relationships** connect two nodes and always have a direction (e.g., “Person1 KNOWS Person2”).

## Cypher Verbs

| Command/Verb | Description                                                    |
| ------------ | -------------------------------------------------------------- |
| `MATCH`      | Finds patterns of nodes and relationships                      |
| `WHERE`      | Filters the matched patterns                                   |
| `RETURN`     | Specifies what you want to retrieve                            |
| `CREATE`     | Inserts data into the graph                                    |
| `MERGE`      | Matches existing data if possible; otherwise, creates new data |
| `SET`        | Updates data                                                   |
| `DELETE`     | Deletes data                                                   |

## Create

### Create Nodes

```cypher
CREATE (n:Person { name: 'Alice', age: 30 })
RETURN n
```

- **CREATE** adds a node labeled `Person` with properties `name` and `age`.
- You can return the node to verify it was created.

### Create a Relationship

```cypher
MATCH (a:Person { name: 'Alice' }), (b:Person { name: 'Bob' })
CREATE (a)-[r:FRIEND_OF]->(b)
RETURN r
```

- First, **MATCH** finds two existing nodes in the database.
- **CREATE** then makes a relationship labeled `FRIEND_OF` from Alice to Bob.
- A relationship’s direction is typically relevant in path queries.

## Read

### Basic MATCH Queries

```cypher
MATCH (n:Person)
RETURN n
```

- Finds all nodes with the label `Person` and returns them.

### Filtering with WHERE

```cypher
MATCH (n:Person)
WHERE n.age > 25
RETURN n.name, n.age
```

- Retrieves only `Person` nodes whose `age` property is greater than 25.
- Returns name and age attributes.

### Matching Relationships

```cypher
MATCH (a:Person)-[r:FRIEND_OF]->(b:Person)
RETURN a.name AS Friend1, b.name AS Friend2
```

- Finds all pairs of people connected by the `FRIEND_OF` relationship.
- Renames returned columns using `AS` for readability.

## Update

### Using SET to Update Properties

```cypher
MATCH (a:Person { name: 'Alice' })
SET a.age = 31
RETURN a
```

- Updates `Alice`’s `age` to 31.

### Add Labels or Properties

```cypher
MATCH (a:Person { name: 'Alice' })
SET a:Employee,
    a.position = 'Developer'
RETURN a
```

- Adds the `Employee` label to `Alice`.
- Adds a new property `position` with the value `"Developer"`.

## Merge Data (MATCH or CREATE in One Step)

### Merge Nodes

```cypher
MERGE (a:Person { name: 'Chris' })
RETURN a
```

- If a node labeled `Person` with `name = 'Chris'` does not exist, it creates it.
- If it does exist, it returns the existing node.

### Merge Relationships

```cypher
MERGE (a:Person { name: 'Alice' })-[rel:FRIEND_OF]->(b:Person { name: 'Bob' })
RETURN a, rel, b
```

- Either finds or creates the entire pattern `(Alice)-[:FRIEND_OF]->(Bob)`.
- Very handy for “upsert”-like operations (update or insert).

## Delete

### Delete Relationships

```cypher
MATCH (a:Person)-[r:FRIEND_OF]->(b:Person)
WHERE a.name = 'Alice' AND b.name = 'Bob'
DELETE r
```

Deletes the `FRIEND_OF` relationship between `Alice` and `Bob`.

### Delete Nodes

**Note:** Nodes can only be deleted if they have no relationships.

```cypher
MATCH (a:Person { name: 'Alice' })
DELETE a
```

If a node has relationships, you need to delete them first, or use **DETACH DELETE**:
```cypher
MATCH (a:Person { name: 'Alice' })
DETACH DELETE a
```
`DETACH DELETE` removes the node and any connected relationships in one step.

## Variables and Aliases

When matching or creating data, you assign node or relationship identifiers (variables) for reuse in the query.

```cypher
MATCH (p:Person { name: 'Alice' })-[r:FRIEND_OF]->(q:Person)
RETURN p, r, q
```

- `p`, `r`, and `q` are variable names referencing the matched nodes or relationships.

## Pattern Matching with Optional Paths

### OPTIONAL MATCH

```cypher
MATCH (a:Person { name: 'Alice' })
OPTIONAL MATCH (a)-[r:FRIEND_OF]->(b:Person)
RETURN a, r, b
```

- If the pattern `(Alice)-[:FRIEND_OF]->(b)` exists, it returns it.
- If no such relationship exists, the query still returns `Alice` with `null` for `r` and `b`.

## Aggregation and Grouping

Cypher supports aggregations (similar to SQL’s SUM, COUNT, etc.). Use **WITH** to chain operations.

### Counting Nodes

```cypher
MATCH (p:Person)
RETURN count(p) AS totalPersons
```

### Grouping and Counting

```cypher
MATCH (p:Person)-[:FRIEND_OF]->(friend:Person)
RETURN p.name AS Name, count(friend) AS NumberOfFriends
ORDER BY NumberOfFriends DESC
```

- Shows how many friends each Person has, sorted from most to fewest.

## Indexes and Constraints

For performance or to enforce data integrity, you may want to create indexes or constraints.

### Create an Index

```cypher
CREATE INDEX personNameIndex FOR (p:Person) ON (p.name)
```

- Speeds up queries matching on `p.name`.

### Create a Uniqueness Constraint

Ensure that no two `Person` nodes share the same `name`:
```cypher
CREATE CONSTRAINT uniquePersonName FOR (p:Person)
REQUIRE p.name IS UNIQUE
```

## Best Practices

- Use Labels and Relationship Types Wisely
    Make labels semantically meaningful. For instance, `Person` or `Movie` and relationships like `ACTED_IN`.
- Create Indexes on Frequently Queried Properties
    If queries frequently filter on `name` or `id`, creating an index or uniqueness constraint will improve performance.
 - Use `MERGE` for Idempotent Data Loading 
    When loading data in bulk, `MERGE` ensures you don’t create duplicate records.
- Keep Data Consistent 
    Consider using constraints to enforce uniqueness and maintain data integrity.

## Resources

- [Neo4j Documentation](https://neo4j.com/docs/)
    - [Neo4j Operations Manual](https://neo4j.com/docs/operations-manual/current/)
    - [Cypher Manual](https://neo4j.com/docs/cypher-manual/current/introduction/)
    - [Cypher Refcard](https://neo4j.com/docs/cypher-refcard/current/) for quick syntax reference
