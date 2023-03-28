## Relational Databases

| Database             | Type        | Pros                                                           | Cons                                          | Use Cases                                             |
| -------------------- | ----------- | -------------------------------------------------------------- | --------------------------------------------- | ----------------------------------------------------- |
| MySQL                | Open-Source | Widely used, easy to set up, rich ecosystem, replication       | Limited scalability, less advanced features   | Web applications, small to medium-sized projects      |
| PostgreSQL           | Open-Source | Extensible, advanced features, good performance, ACID          | Limited scalability, steeper learning curve   | Web applications, analytics, GIS                      |
| Microsoft SQL Server | Commercial  | Scalable, integrated with MS stack, advanced features          | Cost, Windows-centric, licensing complexity   | Enterprise applications, MS stack integration         |
| Oracle Database      | Commercial  | Mature, feature-rich, scalable, advanced analytics             | Cost, complex licensing, steep learning curve | Large-scale applications, ERP, CRM, financial systems |
| SQLite               | Embedded    | Lightweight, serverless, self-contained, easy to use           | Limited scalability, limited concurrency      | Mobile applications, embedded systems, small projects |
| IBM Db2              | Commercial  | Scalable, advanced features, high availability                 | Cost, complex licensing, steep learning curve | Enterprise applications, data warehousing, analytics  |
| MariaDB              | Open-Source | MySQL-compatible, more features, community-driven              | Limited scalability, less mature than MySQL   | Web applications, small to medium-sized projects      |
| Amazon Aurora        | Managed     | MySQL/PostgreSQL-compatible, scalable, managed, fault-tolerant | Cost, limited customizability                 | Cloud applications, data warehousing, analytics       |

## Wide-Column Store

| Database              | Consistency Model     | Pros                                                                             | Cons                                          | Use Cases                                                                                                                                                                  |
| --------------------- | --------------------- | -------------------------------------------------------------------------------- | --------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Apache Cassandra      | Eventually consistent | Highly scalable, high performance, distributed, fault-tolerant                   | Limited querying capabilities, complex setup  | Time-series data, IoT, real-time analytics                                                                                                                                 |
| Apache HBase          | Strong consistency    | Scalable, strong consistency, distributed, Hadoop integration                    | Limited querying, complex setup, high latency | Big data, real-time analytics, Hadoop ecosystem                                                                                                                            |
| ScyllaDB              | Eventually consistent | High performance, Cassandra-compatible, low latency                              | Limited querying, less mature                 | Time-series data, IoT, real-time analytics                                                                                                                                 |
| Amazon DynamoDB       | Tunable consistency   | Managed, scalable, low latency, global replication                               | Limited querying, less flexible, cost         | Serverless applications, gaming, mobile apps                                                                                                                               |
| Google Cloud Bigtable | Strong consistency    | Managed, scalable, low latency, Google Cloud integration                         | Limited querying, cost, less flexible         | IoT, time-series data, analytics, Google Cloud applications                                                                                                                |
| Azure Cosmos DB       | Tunable consistency   | Global distribution, multiple APIs, tunable consistency                          | Cost, complex pricing model                   | Global applications, IoT, big data, real-time analytics                                                                                                                    |
| Apache Accumulo       | Strong consistency    | Cell-level security, Scalability, High availability, Built-in iterator framework |                                               | Government and military applications where security is critical,  Large-scale data processing and analytics                                                                |
| Azure Tables          | Strong consistency    | Scalability, Schema-less design, Integration, Cost-effective                     |                                               | Logging and diagnostics data, User data for web applications, Storing metadata for large-scale applications, Applications that require flexible data storage and retrieval |

In general, Accumulo and Cassandra are more suited for structured and semi-structured data and provide more advanced features such as cell-level security and tunable consistency, while HBase is more focused on real-time data access for large datasets.

## Document databases

| Database          | Type        | Pros                                                       | Cons                                            | Use Cases                                               |
| ----------------- | ----------- | ---------------------------------------------------------- | ----------------------------------------------- | ------------------------------------------------------- |
| MongoDB           | Document    | Schema-less, scalable, high performance, flexible querying | Limited join support, less mature transactions  | Web applications, IoT, content management systems       |
| CouchDB           | Document    | Easy replication, map-reduce, RESTful API, schema-less     | Limited query options, less scalable            | Offline-first applications, web apps, data syncing      |
| Amazon DynamoDB   | Key-value   | Managed, scalable, low latency, global replication         | Limited querying, less flexible, cost           | Serverless applications, gaming, mobile apps            |
| Couchbase         | Document    | Scalable, high performance, rich querying, mobile support  | Steeper learning curve, complex configuration   | Mobile apps, big data, real-time analytics              |
| Azure Cosmos DB   | Multi-model | Global distribution, multiple APIs, tunable consistency    | Cost, complex pricing model                     | Global applications, IoT, big data, real-time analytics |
| Amazon DocumentDB | Document    | MongoDB-compatible, managed, scalable                      | Limited feature set, cost                       | Web applications, analytics, migrating from MongoDB     |
| RavenDB           | Document    | ACID transactions, built-in full-text search, C# focused   | Limited community, less mature                  | .NET applications, CMS, real-time analytics             |
| RethinkDB         | Document    | Real-time push updates, flexible querying, easy clustering | Less mature, smaller community                  | Real-time applications, collaboration tools             |
| LiteDB            | Document    | Lightweight, serverless, single-file, .NET focused         | Limited features, not suited for large datasets | Small .NET applications, embedded systems               |

## Graph Databases

| Database       | Query Language  | Pros                                                                | Cons                                            | Use Cases                                                                               |
| -------------- | --------------- | ------------------------------------------------------------------- | ----------------------------------------------- | --------------------------------------------------------------------------------------- |
| Neo4j          | Cypher          | Highly connected data, powerful querying, ACID transactions         | Steeper learning curve, less mature scalability | Social networks, fraud detection, recommendation engines                                |
| Amazon Neptune | Gremlin, SPARQL | Managed, highly available, scalable, multiple query languages       | Cost, limited customizability                   | Knowledge graphs, social networks, fraud detection                                      |
| ArangoDB       | AQL             | Multi-model (graph, document, key-value), flexible querying         | Less mature, steeper learning curve             | Multi-model applications, social networks, recommendation engines                       |
| OrientDB       | SQL-like        | Multi-model (graph, document), ACID transactions, flexible querying | Smaller community, less mature                  | Multi-model applications, social networks, content management                           |
| JanusGraph     | Gremlin         | Scalable, distributed, pluggable storage backends                   | Complex setup, smaller community                | Social networks, large-scale graphs, real-time analytics                                |
| Virtuoso       | SPARQL, SQL     | Multi-model (graph, relational), high performance, mature           | Steeper learning curve, less user-friendly      | Knowledge graphs, semantic web, linked data                                             |
| TigerGraph     | GSQL            | Scalable, high performance, expressive query language               | Proprietary, cost                               | Social networks, fraud detection, IoT analytics                                         |

## Vector Databases

- [Chroma](https://www.trychroma.com/)
- [DeepsetAI](https://www.deepset.ai/)
- [FAISS](https://github.com/facebookresearch/faiss)
- [Milvus](https://milvus.io/)
- [pgvector](https://github.com/pgvector/pgvector) (for Postgres; Part of [Supabase](https://supabase.com/docs))
- [Pinecone](https://www.pinecone.io/)
- [Qdrant](https://qdrant.tech/)
- [Vespa](https://github.com/vespa-engine/vespa)
- [Weaviate](https://weaviate.io/)

## Search Databases

| Database               | Type                | Pros                                                   | Cons                                   | Use Cases                                                |
| ---------------------- | ------------------- | ------------------------------------------------------ | -------------------------------------- | -------------------------------------------------------- |
| Elasticsearch          | Distributed Search  | Scalable, real-time search, analytics, RESTful API     | Complexity, resource-intensive         | Full-text search, log analytics, monitoring              |
| Apache Solr            | Distributed Search  | Scalable, faceted search, text analysis, mature        | Steeper learning curve, less real-time | Full-text search, analytics, content filtering           |
| Amazon CloudSearch     | Managed Search      | Managed, scalable, easy to set up, AWS integration     | Limited customization, cost            | Full-text search, faceted search, AWS applications       |
| Algolia                | Search-as-a-Service | Fast, hosted, easy to use, rich search features        | Cost, limited data storage             | Instant search, e-commerce, content discovery            |
| Azure Cognitive Search | Managed Search      | Managed, scalable, AI features, Azure integration      | Cost, limited customization            | Full-text search, AI-enhanced search, Azure applications |
| Apache Lucene          | Search Library      | High-performance, extensible, full-text search library | Low-level API, complex setup           | Building custom search solutions                         |
| Typesense              | Search-as-a-Service | Fast, hosted, easy to use, simple setup                | Limited features, smaller community    | Instant search, e-commerce, small-medium applications    |

## Key-Value Databases

| Database        | Scalability          | Pros                                                         | Cons                                            | Use Cases                                             |
| --------------- | -------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ----------------------------------------------------- |
| Redis           | In-memory, sharding  | In-memory, fast, rich data structures, simple setup          | Limited querying, volatile memory by default    | Caching, real-time analytics, message brokering       |
| Amazon DynamoDB | Managed, distributed | Managed, scalable, low latency, global replication           | Limited querying, less flexible, cost           | Serverless applications, gaming, mobile apps          |
| Riak KV         | Distributed          | Highly available, fault-tolerant, easy to scale              | Limited querying, eventual consistency          | IoT, time-series data, content storage                |
| RocksDB         | Embedded             | Embedded, high-performance, storage engine, tunable settings | Limited querying, single-node                   | Storage engine, real-time analytics, edge computing   |
| Leveldb         | Embedded             | Embedded, lightweight, key-value storage library             | Limited querying, single-node                   | Mobile applications, embedded systems                 |
| LMDB            | Embedded             | Embedded, fast, read-optimized, ACID transactions            | Limited querying, single-node                   | Mobile applications, embedded systems                 |
| etcd            | Distributed          | Strong consistency, distributed, reliable, key-value store   | Limited querying, mainly for configuration data | Distributed system coordination, Kubernetes           |
| Aerospike       | Distributed          | High performance, scalable, hybrid memory architecture       | Limited querying, steep learning curve          | Real-time analytics, caching, adtech, fraud detection |

## Other

| Database  | Type                 | Pros                                                   | Cons                                 | Use Cases                                              |
| --------- | -------------------- | ------------------------------------------------------ | ------------------------------------ | ------------------------------------------------------ |
| snowflake | Cloud Data Warehouse | Scalable, pay-as-you-go, multi-cloud, ANSI SQL support | Cost, limited real-time capabilities | Data warehousing, big data analytics, data integration |
