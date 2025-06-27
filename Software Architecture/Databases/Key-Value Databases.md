## Open-Source Key-Value Databases

| Database        | Scalability          | Pros                                                         | Cons                                            | Use Cases                                             |
| --------------- | -------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ----------------------------------------------------- |
| Redis           | In-memory, sharding  | In-memory, fast, rich data structures, simple setup          | Limited querying, volatile memory by default    | Caching, real-time analytics, message brokering       |
| Riak KV         | Distributed          | Highly available, fault-tolerant, easy to scale              | Limited querying, eventual consistency          | IoT, time-series data, content storage                |
| RocksDB         | Embedded             | Embedded, high-performance, storage engine, tunable settings | Limited querying, single-node                   | Storage engine, real-time analytics, edge computing   |
| Leveldb         | Embedded             | Embedded, lightweight, key-value storage library             | Limited querying, single-node                   | Mobile applications, embedded systems                 |
| LMDB            | Embedded             | Embedded, fast, read-optimized, ACID transactions            | Limited querying, single-node                   | Mobile applications, embedded systems                 |
| etcd            | Distributed          | Strong consistency, distributed, reliable, key-value store   | Limited querying, mainly for configuration data | Distributed system coordination, Kubernetes           |

## Proprietary Key-Value Databases

| Database        | Scalability          | Pros                                                         | Cons                                            | Use Cases                                             |
| --------------- | -------------------- | ------------------------------------------------------------ | ----------------------------------------------- | ----------------------------------------------------- |
| Amazon DynamoDB | Managed, distributed | Managed, scalable, low latency, global replication           | Limited querying, less flexible, cost           | Serverless applications, gaming, mobile apps          |
| Aerospike       | Distributed          | High performance, scalable, hybrid memory architecture       | Limited querying, steep learning curve          | Real-time analytics, caching, adtech, fraud detection |
