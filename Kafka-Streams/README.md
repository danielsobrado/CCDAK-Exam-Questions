## Kafka Streams: Building Real-Time Applications and Microservices

Kafka Streams is a client library designed to build real-time applications and microservices, with data both inputted and outputted to Kafka clusters. It marries the simplicity of developing and deploying standard Java and Scala applications with the power of Kafka's server-side capabilities.

### Core Concepts of Kafka Streams

- **Stream Processing**: Fundamentals of processing records in real-time from Kafka topics.
- **Topology**: A graph of stream processors (nodes) connected by streams (edges), defining the flow of data.
- **KStreams and KTables**: KStreams represent a continuous stream of data, while KTables represent a changelog stream, akin to a table in a database.

### Know the Differences between KStream and KTable
- KStream processes each record as an independent event, while KTable treats each record as an update.
- KStream is useful for processing individual events, while KTable is suitable for maintaining the latest value for a given key.
- KStream supports record-by-record transformations, while KTable allows aggregations and joins based on a key.

### Identify the Appropriate Use Cases
- Use KStream when you need to process each record independently, perform stateless transformations, or handle unbounded data.
- Use KTable when you need to perform aggregations, joins, or maintain a materialized view of the latest values for each key.

Stream-table duality:

- **Stream as Table:** A stream can be viewed as a changelog of a table, where each data record in the stream captures a state change of the table. A stream can be turned into a 'real' table by replaying the changelog from the beginning to reconstruct the table.
- **Table as Stream:** A table can be viewed as a snapshot of the latest value for each key in a stream. A table can be turned into a 'real' stream by iterating over each key-value entry in the table.

### Fundamental Components

- **Stream Processor**: A fundamental unit that processes each incoming record, capable of transforming, filtering, or aggregating data streams.
- **State Stores**: Facilitate storing state for stateful operations, enabling functionalities like windowing and interactive queries.

### Permanent State Stores in Kafka Streams:

- **Definition:** Permanent state stores are used in Kafka Streams to persistently store and manage stateful data across reboots and restarts.
- **Types:** Includes key-value stores, window stores, and session stores.
- **Usage:** Essential for operations like aggregations, joins, and windowing which require maintaining state over time.
- **Configuration:** Defined in the Kafka Streams API using `Stores.persistentKeyValueStore()`, `Stores.persistentWindowStore()`, etc.
- **Resource Management:** Always close Iterators after use to prevent resource leaks and Out-Of-Memory (OOM) issues.
- **Recovery:** State stores support fault tolerance by restoring state from changelog topics on restart.
- **Performance:** Performance can be optimized by configuring RocksDB, the default storage engine for state stores.
- **Scaling:** State stores can be scaled across multiple instances using partitioning and sharding.
- **Monitoring:** Monitor metrics related to state stores, such as cache hit rates and store sizes, to ensure efficient operation.

### Key Features

- **Time Windowing**: Execute operations on data within specific time frames, crucial for temporal data analysis.
- **Stateful Operations**: Perform computations that require maintaining a state, such as joins, aggregations, and windowed computations.
- **Exactly-Once Semantics**: Ensure each record is processed exactly once, crucial for fault-tolerant processing.

### Developing with Kafka Streams

#### Building a Stream Processing Application
Steps include setting up the project, defining the topology, and integrating stream processors with Kafka topics.
``` 
StreamsBuilder builder = new StreamsBuilder();
KStream<String, String> textLines = builder.stream("inputTopic");
KTable<String, Long> wordCounts = textLines
    .flatMapValues(textLine -> Arrays.asList(textLine.toLowerCase().split("\\W+")))
    .groupBy((key, word) -> word)
    .count(Materialized.as("countsStore"));
wordCounts.toStream().to("outputTopic", Produced.with(Serdes.String(), Serdes.Long()));
```
#### Application Configuration
Critical configurations for Kafka Streams applications include the application ID, bootstrap servers, and key-value serializers/deserializers.

### Advanced Concepts and Operations

#### Stateless and Stateful Operators
- **Stateless**: Operations like `map`, `filter`, and `foreach` that don't maintain state.
- **Stateful**: Operations such as `join`, `aggregate`, and `count`, which maintain state over time or across keys.

#### Window Types
- **Tumbling Window**: Fixed-size, non-overlapping, time-based windows.
- **Hopping Window**: Fixed-size, overlapping time windows.
- **Sliding Window**: Overlapping windows based on time difference between records.
- **Session Window**: Dynamically sized, non-overlapping windows based on activity sessions.

#### SerDes and Streams DSL
- **SerDes**: Serialization and Deserialization frameworks essential for data interpretation in Kafka Streams.
- **Streams DSL**: High-level domain-specific language to define stream processing topologies, including `KStream`, `KTable`, and `GlobalKTable`.

#### Join Operations

Using the Streams API, the following join operations are supported, with joins being either windowed or non-windowed depending on the operands:

- **KStream-to-KStream (windowed):** Supports inner join, left join, and outer join.
- **KTable-to-KTable (non-windowed):** Supports inner join, left join, and outer join.
- **KStream-to-KTable (non-windowed):** Supports inner join and left join. Outer join is not supported.
- **KStream-to-GlobalKTable (non-windowed):** Supports inner join and left join. Outer join is not supported.

### Message Delivery Guarantees

Kafka supports three types of message delivery guarantees:

- **At-most-once:** Every message is persisted in Kafka at most once. Message loss is possible if the producer doesn't retry on failures.
- **At-least-once:** Every message is guaranteed to be persisted in Kafka at least once. There is no chance of message loss, but messages can be duplicated if the producer retries after the message is already persisted.
- **Exactly-once:** Every message is guaranteed to be persisted in Kafka exactly once without any duplicates or data loss, even in the event of broker failures or producer retries.

Exactly-once processing can be achieved for Kafka-to-Kafka workflows using the Kafka Streams API. For Kafka-to-sink workflows, an idempotent consumer is required.

Stream processing applications written with the Kafka Streams library can enable exactly-once semantics by setting the `processing.guarantee` configuration to `exactly_once` (the default value is `at_least_once`). This change requires no code modifications.

### Co-Partitioning

Co-partitioning is a concept in Kafka where two or more topics have their partitions aligned in such a way that the same partition numbers across these topics contain related data. This alignment is crucial for operations that involve joining data streams, ensuring that related data from different topics can be processed together efficiently. Here are the key rules and considerations for achieving co-partitioning in Kafka:

1. **Same Number of Partitions**: Co-partitioned topics must have the same number of partitions. This ensures that data which is logically related and keyed similarly ends up in the corresponding partition across these topics.

2. **Consistent Partitioning Strategy**: To ensure that related messages from different topics land in the corresponding partitions, a consistent partitioning strategy must be used. This often involves ensuring that messages are produced with the same key and that the default partitioner or a custom partitioner is used consistently across these topics.

3. **Parallel Consumer Processing**: When consuming from co-partitioned topics, ensure that the consumer groups are configured to consume in parallel from all the related partitions. This is typically handled naturally by Kafka's consumer group mechanism when the topics are consumed together.

4. **Kafka Streams and KTable**: In Kafka Streams, co-partitioning is essential when performing join operations between KStreams and KTables or between KStreams themselves. Kafka Streams applications will automatically co-partition the data of joined streams by repartitioning them as needed, which may introduce additional processing steps and topics.

5. **Use Case for Co-Partitioning**: Co-partitioning is often used in scenarios requiring data aggregation or join operations across different data streams that share a logical relationship, such as aggregating user click events with user profile information for real-time analytics.

6. **Management and Maintenance**: When scaling out (adding more partitions to topics), ensure that all co-partitioned topics are scaled consistently. This might require manual intervention or custom tooling, as Kafka does not automatically manage the partition count across topics to maintain co-partitioning.

7. **Producing to Co-Partitioned Topics**: When producing messages to co-partitioned topics, ensure that the keys used for partitioning are consistent and that messages destined to be joined together use the same keys.

### Parallel processing

Kafka Streams scales by allowing multiple threads of execution within one instance of the application and by supporting load balancing between distributed instances of the application. You can run the Streams application on one machine with multiple threads or on multiple machines; in either case, all active threads in the application will balance the data processing work.

The Streams engine parallelizes the execution of a topology by splitting it into tasks. The number of tasks is determined by the Streams engine and depends on the number of partitions in the topics that the application processes. Each task is responsible for a subset of the partitions: the task subscribes to those partitions and consumes events from them. For every event it consumes, the task executes all the processing steps that apply to this partition in order before eventually writing the result to the sink. These tasks are the basic unit of parallelism in Kafka Streams, as each task can execute independently of others.

To achieve the maximum parallelism in a Kafka Streams application, you can set the `num.stream.threads` configuration parameter to the desired parallelism, **not necessarily the number of partitions**.

### Repartitioning

When Kafka Streams requires repartitioning, it:
1. Writes the repartitioned data to a new topic with new keys and partitions.
2. Creates a new set of tasks to read and process events from the new topic.

This process divides the topology into two subtopologies, each with its own tasks. The second subtopology depends on the output of the first.

The two sets of tasks can still run independently and in parallel because:
- The first set writes data to the new topic at its own pace.
- The second set consumes and processes events from the new topic independently.

There is no communication or shared resources between the tasks, allowing them to run on separate threads or servers.

## Understand the Available Operations
- KStream operations:
 - `map`: Applies a one-to-one transformation to each record in the stream.
   Example:
   ```java
   KStream<String, Integer> doubled = stream.map((key, value) -> new KeyValue<>(key, value * 2));
   ```
 - `filter`: Filters records based on a predicate.
   Example:
   ```java
   KStream<String, Integer> filtered = stream.filter((key, value) -> value > 100);
   ```
 - `flatMap`: Applies a one-to-many transformation to each record, flattening the result into a new stream.
   Example:
   ```java
   KStream<String, String> words = stream.flatMapValues(value -> Arrays.asList(value.split("\\s+")));
   ```
 - `branch`: Splits a stream into multiple streams based on predicates.
   Example:
   ```java
   KStream<String, Integer>[] branches = stream.branch((key, value) -> value > 100, (key, value) -> value <= 100);
   ```
 - `merge`: Merges multiple streams into a single stream.
   Example:
   ```java
   KStream<String, Integer> merged = stream1.merge(stream2);
   ```
- KTable operations:
 - `aggregate`: Performs an aggregation on the records of a KTable.
   Example:
   ```java
   KTable<String, Long> counts = table.groupByKey().aggregate(() -> 0L, (aggKey, newValue, aggValue) -> aggValue + newValue);
   ```
 - `reduce`: Combines the records of a KTable based on a reducer function.
   Example:
   ```java
   KTable<String, Integer> reduced = table.groupByKey().reduce((value1, value2) -> value1 + value2);
   ```
 - `join`: Joins two KTables based on their keys.
   Example:
   ```java
   KTable<String, String> joined = table1.join(table2, (value1, value2) -> value1 + "-" + value2);
   ```
 - `groupBy`: Groups the records of a KTable based on a new key.
   Example:
   ```java
   KTable<String, Integer> grouped = table.groupBy((key, value) -> value % 10);
   ```
 - `count`: Counts the number of records in a KTable.
   Example:
   ```java
   KTable<String, Long> counts = table.groupByKey().count();
   ```

## Be Familiar with the Syntax and APIs
- Creating a KStream:
 ```java
 KStream<String, String> stream = builder.stream("input-topic");
 ```
- Creating a KTable:
 ```java
 KTable<String, String> table = builder.table("input-topic");
 ```
- Performing transformations:
 ```java
 KStream<String, Integer> transformed = stream.mapValues(value -> value.length());
 ```
- Aggregating data:
 ```java
 KTable<String, Long> aggregated = stream.groupByKey().count();
 ```
- Joining streams and tables:
 ```java
 KStream<String, String> joined = stream.leftJoin(table, (streamValue, tableValue) -> streamValue + "-" + tableValue);
 ```

### Best Practices for Kafka Streams

- **State Management**: Leverage state stores and interactive queries efficiently to manage state.
- **Scaling and Performance**: Apply strategies to scale applications horizontally and optimize performance through tuning and configuration adjustments.
- It is recommended to store Kafka Streams application state on a **persistent volume** to minimize recovery time by only restoring the missing state upon failure or restart.

Kafka Streams offers a comprehensive set of tools and functionalities for building robust, real-time streaming applications, leveraging Kafka's strengths in handling vast streams of data efficiently and reliably.
