## KSQL Overview for CCDAK

KSQL, known as ksqlDB in its more recent versions, is a stream processing framework that enables real-time data processing against Apache Kafka. It uses a SQL-like language, making it accessible to developers familiar with SQL.

### Key Concepts

- **Stream Processing**: Understand how KSQL processes data in real-time, transforming, enriching, and querying data streams.
- **KSQL Syntax**: Familiarity with the SQL-like syntax for creating streams, tables, and performing queries.
- **Streams vs. Tables**: Grasp the difference between a stream (an immutable sequence of records) and a table (a mutable, stateful entity).

ksqlDB supports exactly-once processing, ensuring correct results even in the event of failures such as machine crashes. Developers can configure this behavior using the `processing.guarantee` setting.

Idle servers in a ksqlDB cluster consume minimal resources. When running a query against a Kafka topic with multiple partitions, only the servers corresponding to the number of partitions perform the actual work.

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

The `DESCRIBE EXTENDED` statement in ksqlDB provides detailed information about a stream or table, including its serialization format. Checking the compatibility between the `VALUE_FORMAT` of a stream and the format of records in a topic can help diagnose deserialization issues.

### Best Practices

- **Query Optimization**: Tips for optimizing query performance, such as minimizing the use of repartitioning and choosing appropriate keys for joins.
- **Monitoring and Troubleshooting**: Basic practices for monitoring KSQL performance and troubleshooting common issues.

### Real-world Application Scenarios

- **Real-Time Analytics**: Use KSQL for instant analytics on streaming data, such as calculating average values, counts, or processing time-series data.
- **Anomaly Detection**: Implement anomaly detection systems by processing and analyzing data in real-time, identifying outliers or unusual patterns.
- **Data Enrichment**: Enrich streaming data by joining it with static datasets or additional streams, adding value and context to the data.

### Headless mode

ksqlDB offers two deployment modes:

1. **Headless ksqlDB deployment (Application Mode):**
   - This is the recommended mode for production deployments.
   - You write your queries in a SQL file and start ksqlDB server instances with this file as an argument.
   - Each server instance reads the SQL file, compiles the ksqlDB statements into Kafka Streams applications, and starts executing the generated applications.

2. **Interactive ksqlDB deployment:**
   - In this mode, you interact with the ksqlDB servers through a REST API.
   - You can interact directly using REST clients, the ksqlDB CLI, or Confluent Control Center.
   - This mode is suitable for development, testing, and exploratory purposes.

`ksqlDB` supports a locked-down, "headless" deployment mode that disables interactive access to the `ksqlDB` cluster. This mode is useful in production environments where you want to ensure that only predefined queries are executed and prevent direct user interaction with the cluster.

In a typical workflow:
1. A team of users develops and tests their queries interactively on a shared testing `ksqlDB` cluster using the CLI.
2. When ready for production deployment, the verified queries are version-controlled and stored in a `.sql` file.
3. The `.sql` file is provided to the `ksqlDB` servers during startup using either:
   - The `--queries-file` command-line argument, or
   - The `ksql.queries.file` setting in the `ksqlDB` configuration file.

When a `ksqlDB` server is running with a predefined script (`.sql` file), it automatically disables its REST endpoint and interactive use. This lockdown mechanism ensures that the server only executes the predefined queries and prevents any direct interaction with the production `ksqlDB` cluster.
In headless mode, ksqlDB stores metadata in the config topic. The config topic stores the ksqlDB properties provided to ksqlDB when the application was first started. 
ksqlDB uses these configs to ensure that your ksqlDB queries are built compatibly on every restart of the server.

### ksqlDB In-Depth

#### 1. ksqlDB Overview
- ksqlDB is an event streaming database that allows building event streaming applications using SQL-like syntax.
- It uses Kafka Streams under the hood, so understanding Kafka Streams concepts is beneficial when using ksqlDB.
- ksqlDB runs continuous queries that constantly evaluate incoming events from Kafka topics and can persist results back to Kafka or return them to clients.
- ksqlDB offers a powerful way to build event streaming applications using familiar SQL syntax, making it accessible to a wider audience beyond Java developers.
- Question: How does ksqlDB handle state management and fault tolerance?
  - ksqlDB leverages Kafka's fault-tolerant and distributed architecture for state management and fault tolerance.
  - It uses Kafka topics to store intermediate state and supports exactly-once semantics through transaction support.

#### 2. Streams and Tables
- ksqlDB supports creating `STREAM`s and `TABLE`s, which have similar semantics to KStream and KTable in Kafka Streams.
  - A `STREAM` represents an unbounded sequence of independent events, where records with the same key are not related.
  - A `TABLE` is an update stream, where records with the same key are considered updates to previous records with that key.
- Streams and tables can be created from a Kafka topic using a `CREATE STREAM` or `CREATE TABLE` statement.
- The `WITH` clause is used to specify properties like the Kafka topic, number of partitions, and serialization format.
- Key columns are optional for streams but required for tables. They are defined using the `KEY` keyword.
- **For tables, null keys are not allowed and will cause the record to be dropped**. For streams, null keys are allowed.
- **In tables, null values are considered tombstones and mark the record for deletion** . In streams, null values have no special meaning.
- Question: Can you create a stream or table without an underlying Kafka topic?
  - No, every stream or table in ksqlDB must be backed by a Kafka topic.
  - The Kafka topic can be pre-existing, or ksqlDB can create it automatically based on the stream or table definition.

#### 3. Query Types
1. **Source queries**:
   - Create a `STREAM` or `TABLE` backed by a Kafka topic.
   - Defined using `CREATE STREAM` or `CREATE TABLE` statements.
   - Bring data from a Kafka topic into ksqlDB for further processing.

2. **Persistent queries**:
   - Select columns from a source query and persist results to a Kafka topic.
   - Defined using `CREATE STREAM AS SELECT` or `CREATE TABLE AS SELECT` statements.
   - Results are stored in a Kafka topic and can be shared by multiple clients.
   - Continuously process incoming events and update the result topic.

3. **Push queries**:
   - Select a subset of columns from a persistent query and stream results directly to the client.
   - Defined using a `SELECT` statement with the `EMIT CHANGES` clause.
   - Run indefinitely until terminated by the client.
   - Results are not persisted to a topic; they are returned to the client in real-time.

4. **Pull queries**:
   - Execute once and terminate, returning a result set to the client.
   - Defined using a regular `SELECT` statement without the `EMIT CHANGES` clause.
   - Have some limitations on supported SQL statements (e.g., no `JOINs`, `GROUP BY`, or `WINDOW` clauses).
   - Results are not persisted to a topic and are not shared; identical queries from different clients are executed independently.
   - Question: Can pull queries be executed on any stream or table?
     - Pull queries **can only be executed on materialized streams or tables that have a defined primary key**.
     - They cannot be executed on non-materialized streams or tables without a primary key.

- Question: What happens if a persistent query fails or is terminated unexpectedly?
  - If a persistent query fails, ksqlDB will automatically restart it from the last committed offset.
  - The query will resume processing events from where it left off, ensuring no data loss.
  - If a query is terminated unexpectedly (e.g., due to server failure), it will be automatically restarted on another ksqlDB server in the cluster.

#### 4. Schema Registry Integration
- ksqlDB integrates with Schema Registry for Avro, Protobuf, and JSON Schema serialization formats.
- Streams and tables inherit key and value formats from their backing topics or source streams/tables.
- Formats can be changed using a `WITH` clause when defining a stream or table.
- When using a schema-based format, column names and types can be omitted in the definition, as they are inferred from the schema.
- ksqlDB automatically registers schemas with Schema Registry when creating streams or tables based on persistent queries.
- The `ksql.schema.registry.url` property must be set to the Schema Registry URL to enable schema integration.
- Question: What happens if the schema of a topic changes and doesn't match the schema registered in ksqlDB?
  - If the schema of a topic changes and doesn't match the registered schema, ksqlDB will fail to deserialize the data and will report an error.
  - To handle schema changes, you need to update the stream or table definition in ksqlDB to match the new schema.
  - ksqlDB supports schema evolution using Confluent Schema Registry's compatibility rules (e.g., backward, forward compatibility).

#### 5. Aggregations and Functions
- ksqlDB provides built-in aggregation functions like `COUNT`, `SUM`, `AVG`, `MIN`, and `MAX`.
- Aggregations are performed using the `GROUP BY` clause in a persistent query.
- Aggregation results are stored in a table, which is backed by a Kafka topic.
- ksqlDB supports user-defined functions (UDFs) and user-defined aggregation functions (UDAFs) for custom processing logic.
- ksqlDB has a limitation in that it doesn't support structured keys, preventing the creation of a stream from a windowed aggregate.
- Question: Can you perform aggregations on windowed data in ksqlDB?
  - Yes, ksqlDB supports windowed aggregations using the `WINDOW` clause.
  - Windows can be defined based on time (e.g., tumbling, hopping, session windows) or custom window boundaries.
  - Windowed aggregations allow grouping and aggregating data within specific time intervals.

#### 6. Joins
- **Stream-Stream joins**:
  - Create new streams by joining two streams.
  - Requires co-partitioning (same key type and number of partitions).
  - Supports `INNER`, `LEFT OUTER`, `RIGHT OUTER`, and `FULL OUTER` joins.
  - Defined using a `CREATE STREAM AS SELECT` statement with a `JOIN` clause.

- **Stream-Table joins**:
  - Enrich a stream with data from a table.
  - Requires co-partitioning.
  - Performs a lookup in the table for each record in the stream.
  - Defined using a `CREATE STREAM AS SELECT` statement with a `JOIN` clause.

- **Table-Table joins**:
  - Perform joins between tables using primary keys or foreign keys.
  - Primary key joins require co-partitioning.
  - Foreign key joins allow joining on a non-primary key field in the value of one table.
  - Defined using a `CREATE TABLE AS SELECT` statement with a `JOIN` clause.

- While Stream-KGlobalTable offers significant convenience and flexibility for join operations by eliminating the co-partitioning requirement, it also means that each application instance requires enough memory to hold the entire dataset.
- Foreign Key Joins: Do not require co-partitioning based on the primary key of each table. 

- Question: What is the default join window size for stream-stream joins in ksqlDB?
  - The default join window size is 24 hours.
  - You can specify a custom join window size using the `WITHIN` clause in the `JOIN` statement.
  - The join window determines the maximum time difference between the joining records.

- Question: Can you perform a left join between a stream and a table in ksqlDB?
  - Yes, you can perform a left join between a stream and a table using the `LEFT JOIN` syntax.
  - The stream is the left side of the join, and the table is the right side.
  - For each record in the stream, ksqlDB will perform a lookup in the table based on the join key.
  - If a matching record is found in the table, the join result will include fields from both the stream and the table.
  - If no matching record is found in the table, the join result will include fields from the stream and NULL values for the fields from the table.

#### 7. Nested Data Handling
- ksqlDB supports querying nested data structures using the `STRUCT` data type.
- `STRUCT` is used to model nested schemas in streams or tables.
- Nested fields are accessed using the `->` operator to dereference objects and drill down to specific fields.
- Array elements can be accessed using array indexing, e.g., `field->array[index]`.
- Map values can be accessed using `field->map['key']`.
- Nested data can be flattened using the `EXPLODE` function to create separate records for each element in an array or map.
- Question: Can you perform joins on nested fields in ksqlDB?
  - Yes, you can perform joins on nested fields by accessing them using the `->` operator.
  - For example, you can join two streams based on a nested field using `JOIN ON stream1->nested->field = stream2->nested->field`.

#### 8. Deployment and Scaling
- ksqlDB can be deployed in standalone mode or as a cluster of servers.
- In standalone mode, a single ksqlDB server is responsible for processing all queries and interacting with the Kafka cluster.
- In cluster mode, multiple ksqlDB servers work together to distribute the processing load and provide fault tolerance.
- ksqlDB leverages Kafka's distributed architecture to scale horizontally by adding more servers to the cluster.
- Persistent queries are automatically distributed across the available ksqlDB servers based on the partitioning of the input topics.
- ksqlDB supports interactive and headless (non-interactive) deployment modes.
  - Interactive mode allows users to submit queries and receive results through a REST API or command-line interface (CLI).
  - Headless mode is suitable for production deployments, where queries are predefined and executed automatically without user interaction.
- Question: How does ksqlDB handle service discovery in a clustered deployment?
  - In a clustered deployment, ksqlDB servers use Apache ZooKeeper for service discovery and coordination.
  - Each ksqlDB server registers itself with ZooKeeper and maintains a persistent session.
  - Clients (e.g., CLI, REST API) discover the available ksqlDB servers by querying ZooKeeper.
  - If a ksqlDB server fails or becomes unavailable, ZooKeeper notifies the other servers, and the workload is redistributed among the remaining servers.

#### 9. Monitoring and Troubleshooting
- ksqlDB exposes metrics through JMX (Java Management Extensions) for monitoring the health and performance of the system.
- Key metrics to monitor include:
  - `messages-consumed-per-sec`: The number of messages consumed per second by ksqlDB.
  - `messages-produced-per-sec`: The number of messages produced per second by ksqlDB.
  - `error-rate`: The number of errors occurred per second in ksqlDB.
  - `num-active-queries`: The number of active queries running in ksqlDB.
- ksqlDB provides a REST API for managing and monitoring the system, including retrieving cluster information, listing streams and tables, and executing queries.
- The ksqlDB CLI can be used to interact with the system, execute queries, and view query results and logs.
- If ksqlDB doesn't clean up its internal topics make sure that your Kafka cluster is configured with the property delete.topic.enable=true.
- Logs can be configured and monitored to troubleshoot issues and track the execution of queries.
- ksqlDB supports configurable error handling, such as deserialization errors and production exceptions, through the use of dead letter queues (DLQs).
- Question: How can you monitor the resource utilization of ksqlDB servers?
  - ksqlDB exposes metrics for CPU, memory, and network utilization through JMX.
  - You can use tools like JConsole, VisualVM, or Prometheus to scrape and visualize these metrics.
  - Monitoring resource utilization helps in identifying performance bottlenecks and capacity planning.

- Question: What are the best practices for troubleshooting ksqlDB queries?
  - Use the `DESCRIBE` and `EXPLAIN` statements to understand the schema and execution plan of a query.
  - Monitor the ksqlDB logs for error messages and stack traces.
  - Use the `SHOW QUERIES` statement to view the status and metrics of running queries.
  - Enable query logging by setting the `ksql.logging.processing.topic.auto.create` and `ksql.logging.processing.stream.auto.create` properties to `true`.
  - Investigate the Kafka consumer group lag for the input topics to identify if the queries are falling behind in processing.
  - Use the ksqlDB REST API or CLI to execute `SELECT` statements and verify the query results.

