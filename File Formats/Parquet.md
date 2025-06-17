Apache Parquet is an open-source, columnar file format designed for efficient data storage and analytics.

The main advantage is that it offers considerable storage savings and performance improvements compared to CSV files.

## Considerations

- The file format is optimized for append-only or immutable datasets.
- Allows adding new columns and merging schemas across files.

## How Parquet Works

- **Columnar Storage**  
  Data is stored per column rather than per row. Each column is grouped, compressed, and encoded independently—ideal for analytical queries that access select fields.
- **Row Groups & Column Chunks**
  Files are split into row groups, which contain column chunks further divided into pages. This structure supports parallel read/write and efficient I/O.
- **Efficient Encoding & Compression**
  Parquet applies column-specific encodings like dictionary, bit-packing, and run-length encoding, followed by compression codecs (Snappy, Gzip, Zstd, LZ4).
- **Embedded Metadata**  
  The schema and column statistics (min/max, counts) are stored in the footer. This self-describing feature enables tools to understand data structure and skip irrelevant portions.
- **Splittable & Parallel Friendly**  
  Because of structured metadata and row groups, Parquet files can be processed in parallel by systems like Spark, Hadoop, or BigQuery.

## Basic Architecture

### Storage Layer

Data is stored in `.parquet` files, while metadata is stored in `.json` files:
```
\my_table\1.parquet
\my_table\2.parquet
\my_table\3.parquet
\my_table\_log\metadata1.json
\my_table\metadata2.json
```
The data is stored in a file system (File System, AWS S3, GCP Storage, Azure Data Lake, Apache Iceberg, etc.)

## Catalog Service

Catalog services provide centralized **data governance**, **cataloging**, and **multi-engine metadata management**.

**Examples**

- [Databricks Unity Catalog](https://www.databricks.com/product/unity-catalog)
- [Apache Polaris](https://polaris.apache.org/)

## Compute Engines

Since Parquet is an open file format it can be processed by various data engines.

Because of the structure of the metadata and row groups, Parquet files can be processed in parallel my different systems.

| Engine                                               | Architecture & Deployment                                                                                                    | Strengths & Use Cases                                                                     | Scale & Performance                                                            | Notable Limitations                                              |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ---------------------------------------------------------------- |
| [DuckDB](https://duckdb.org/)                        | In-process, embedded, column‑oriented OLAP engine; single binary, zero dependencies ([en.wikipedia.org][1], [duckdb.org][2]) | Ideal for local analytics, data science, ETL pipelines, embedded use in apps or notebooks | Excellent for medium datasets (\<RAM), vectorized processing; single‑node only | Not distributed; limited for petabyte-scale workloads            |
| [Apache Spark](https://spark.apache.org/)            | Distributed cluster engine; uses RDD/DataFrame API; deploys on YARN/K8s                                                      | General-purpose: batch processing, real-time streaming, machine learning, graph jobs      | Scales horizontally; in-memory caching enables fast processing on large data   | Overhead for small jobs; needs tuning for memory                 |
| [Apache Impala](https://impala.apache.org/)          | MPP distributed SQL engine for Hadoop, written in C++/Java                                                                   | Low-latency SQL queries on HDFS/S3/HBase; great for BI on Hadoop                          | Fast query response; optimized for moderate-size haystacks                     | Requires dedicated cluster, works best with sufficient resources |
| [Apache Hadoop](https://hadoop.apache.org/)          | Core distributed storage (HDFS) + MapReduce; batch‑processing framework                                                      | Massive-scale batch analytics, ETL, custom MapReduce jobs                                 | Petabyte-scale processing over many nodes                                      | High latency; not suited for interactive querying                |
| [Apache Hive](https://hive.apache.org/)              | SQL layer over Hadoop using MapReduce/Tez/Spark                                                                              | Data warehousing on Hadoop; long-running queries; batch ETL                               | Scales massively; supports ACID, schema-on-read, multiple formats              | Interactive performance laggy; needs proper engine setup         |
| [Snowflake](https://www.snowflake.com/)              | Cloud-native, fully managed MPP data warehouse                                                                               | Enterprise analytics, multi-cloud workloads, data sharing                                 | Virtually unlimited scaling, automatic tuning                                  | Proprietary; cost tied to usage & compute                        |
| [Google BigQuery](https://cloud.google.com/bigquery) | Serverless, distributed DWH on GCP, uses Dremel architecture                                                                 | Ad-hoc analytics, data lakehouse on petabytes, linear scalability                         | High-performance, massively parallel                                           | Serverless model may reduce control; charges per scanned data    |
| [Amazon Athena](https://aws.amazon.com/athena/)      | Serverless Presto-based SQL engine on S3                                                                                     | Querying S3 directly without infrastructure; ad-hoc analytics                             | Scales transparently; pay per query/data scanned                               | Performance depends on file layout/partitioning                  |
| [Presto](https://prestodb.io/)                       | Distributed MPP SQL engine across varied data sources, written in Java                                                       | Interactive BI across multiple sources (HDFS, S3, DBs)                                    | Low-latency, optimized for interactive queries                                 | Needs cluster setup; memory intensive at scale                   |
## Alternatives

- [DuckLake](https://duckdb.org/2025/05/27/ducklake.html)
	- stores the Metadata in an relational database (PostgreSQL format) instead of JSON files for improved performance
	- data is still persisted in Parquet format
