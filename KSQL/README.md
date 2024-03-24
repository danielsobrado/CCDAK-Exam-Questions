## KSQL Overview for CCDAK

KSQL, known as ksqlDB in its more recent versions, is a stream processing framework that enables real-time data processing against Apache Kafka. It uses a SQL-like language, making it accessible to developers familiar with SQL.

### Key Concepts

- **Stream Processing**: Understand how KSQL processes data in real-time, transforming, enriching, and querying data streams.
- **KSQL Syntax**: Familiarity with the SQL-like syntax for creating streams, tables, and performing queries.
- **Streams vs. Tables**: Grasp the difference between a stream (an immutable sequence of records) and a table (a mutable, stateful entity).

### Essential Components

- **KSQL Server**: The server that runs KSQL queries. It interacts with Kafka to execute stream processing applications.
- **KSQL CLI**: The command-line interface to interact with KSQL Server, useful for executing commands and queries.

#### Configuration Details

- **`auto.offset.reset`**: By default, it is set to "latest", meaning that streams start reading messages that arrive after their creation. Can be changed to "earliest" to read from the beginning.
- **`ksql.streams.state.dir`**: Defines the directory for RocksDB storage used by stateful operations.

#### Advanced Stream Creation

- **Existing Topic**: Streams can be created from existing Kafka topics, with support for various data formats.
- **Stream Count/Join**: Creating streams through aggregations or joins on existing streams.
- **Persistent Streaming Query**: Continuously running queries that write results to a new stream or table.

#### Additional Stream Features

- **ROWTIME and ROWKEY**: Every KSQL stream includes `ROWTIME` and `ROWKEY` fields by default, representing the message timestamp and the key of the topic, respectively.
- **Re-Keying and Materializing**: Techniques for re-keying a stream and creating a materialized view to support efficient querying.

#### Query Types

- **Pull Queries**: Execute against materialized views with limitations on the WHERE clause to key column constraints.
- **Push Queries**: Subscribe to future query results with `EMIT CHANGES`, allowing real-time streaming of query results.

#### Merging Streams

- Demonstrates how to combine data from multiple streams into a single stream, useful for aggregating similar data from different sources.

#### Windowing Functions

- **TOPK/TOPKDISTINCT**: Functions for retrieving the top K values within a window, useful for analytics and leaderboards.
- **GEO_DISTANCE**: A function for calculating geospatial distances, enabling location-based filtering and analysis.

### Important Commands and Concepts

- **Creating Streams and Tables**: How to define streams and tables over Kafka topics to start processing data.
    ```sql
    CREATE STREAM stream_name (column_name data_type, ...) WITH (kafka_topic='topic_name', value_format='format', ...);
    ```
- **Data Processing**: Understanding of how to perform data transformations, aggregations, and joins using KSQL.
- **Windowing**: Knowledge of windowing functions to process data in time-bound chunks.
- **User-Defined Functions (UDFs)**: Familiarity with using and possibly creating custom functions for specialized processing needs.

### Best Practices

- **Query Optimization**: Tips for optimizing query performance, such as minimizing the use of repartitioning and choosing appropriate keys for joins.
- **Monitoring and Troubleshooting**: Basic practices for monitoring KSQL performance and troubleshooting common issues.

### Real-world Application Scenarios

- **Real-Time Analytics**: Use KSQL for instant analytics on streaming data, such as calculating average values, counts, or processing time-series data.
- **Anomaly Detection**: Implement anomaly detection systems by processing and analyzing data in real-time, identifying outliers or unusual patterns.
- **Data Enrichment**: Enrich streaming data by joining it with static datasets or additional streams, adding value and context to the data.

