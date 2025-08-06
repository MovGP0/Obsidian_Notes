## Open-Source Relational Databases

| Database                                          | Type                  | Notes                                                                       | DB-Model               | NuGet                                              |
| ------------------------------------------------- | --------------------- | --------------------------------------------------------------------------- | ---------------------- | -------------------------------------------------- |
| MySQL                                             | Open-Source           | Row-based engine (InnoDB), good for transactional apps                      | OLTP                   |                                                    |
| Postgres/PostgreSQL                               | Open-Source           | Primarily OLTP, but supports analytics and extensions (e.g. Citus for OLAP) | OLTP<br>(OLAP-capable) |                                                    |
| SQLite                                            | Embedded              | Embedded row-store, single-user concurrency model                           | OLTP                   | *use MySQL Connector*                              |
| [MariaDB](https://mariadb.org/)                   | Open-Source           | MySQL-compatible, more features, community-driven                           | OLTP                   | [DuckDB*](https://www.nuget.org/packages?q=duckdb) |
| [DuckDB](https://duckdb.org/)                     | Embedded, Open-Source | In-process OLAP engine, columnar store optimized for analytics              | OLAP                   |                                                    |
| [dolt](https://github.com/dolthub/dolt)           | Open-Source           | OLTP-style MySQL-compatible engine with Git-like versioning                 | OLTP + Version Control |                                                    |
| [doltgres](https://github.com/dolthub/doltgresql) | Open-Source           | PostgreSQL-compatible versioned DB; OLTP engine                             | OLTP + Version Control |                                                    |
## Proprietary Relational Databases

| Database             | Type                 | Notes                                                                                      | DB-Model            | NuGet |
| -------------------- | -------------------- | ------------------------------------------------------------------------------------------ | ------------------- | ----- |
| Microsoft SQL Server | Commercial           | Core engine is row-store; supports OLAP via columnstore indexes and SSAS                   | OLTP (OLAP-capable) |       |
| Oracle Database      | Commercial           | Mature OLTP engine with integrated OLAP features (e.g., Oracle OLAP, analytical functions) | OLTP (OLAP-capable) |       |
| IBM Db2              | Commercial           | Traditionally OLTP; OLAP via Db2 Warehouse and BLU Acceleration                            | OLTP (OLAP-capable) |       |
| Amazon Aurora        | Managed              | Cloud-native OLTP; supports MySQL/PostgreSQL protocols                                     | OLTP                |       |
| snowflake            | Cloud Data Warehouse | Cloud-native OLAP, columnar, designed for analytics workloads                              | OLAP                |       |

## Notes

| DB-Model | Description                                                            |
| -------- | ---------------------------------------------------------------------- |
| OLTP     | Optimized for transactional workloads — fast reads/writes per row      |
| OLAP     | Optimized for analytical workloads — aggregation across large datasets |
