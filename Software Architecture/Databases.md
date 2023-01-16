## Relational Databases

| Database             | Description                     |
| -------------------- | ------------------------------- |
| Microsoft SQL Server |                                 |
| Oracle               |                                 |
| MySQL                |                                 |
| MariaDB              |                                 |
| PostgreSQL           |                                 |
| SQLite               | Library; uses PostgreSQL syntax |
| Access               | do not use                      |
| IBM DB2              |                                 |

## Wide-Column Store

| Database         | Description                                                                                                   |
| ---------------- | ------------------------------------------------------------------------------------------------------------- |
| Apache Cassandra | BigTable; high availability and scalability; multiple consistency levels; Write-Optimized                     |
| Apache Accumulo  | BigTable based on Hadoop Distributed File System (HDFS) and Zookeeper; cell-level security and access control |
| Apache HBase     | BigTable; real-time streaming, OLAP and OLTP systems                                                          |
| Azure Tables     |                                                                                                               |

In general, Accumulo and Cassandra are more suited for structured and semi-structured data and provide more advanced features such as cell-level security and tunable consistency, while HBase is more focused on real-time data access for large datasets.

## Document databases

| Database          | Description  |
| ----------------- | ------------ |
| mongoDB           |              |
| CouchDB           |              |
| Amazon DynamoDB   |              |
| Couchbase         |              |
| Azure Cosmos DB   |              |
| Amazon DocumentDB |              |
| RavenDB           |              |
| RethinkDB         |              |
| LiteDB            | .NET Library |

## Graph Databases

| Database |
| -------- |
| ArangoDB |
| OrientDB |

## Search Databases

| Database      | Description |
| ------------- | ----------- |
| elasticsearch |             |
| Apache Solr   |             |

## Other

| Database  | Description                    |
| --------- | ------------------------------ |
| redis     | In-Memory cache/message broker |
| snowflake |                                |
| Aerospike |                                |
| RocksDB   | Key-Value                      |
