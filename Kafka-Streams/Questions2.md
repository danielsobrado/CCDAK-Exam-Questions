## Question 11

In a Kafka Streams application, where are the processing topology configurations stored?

- A. In a special Kafka topic named `streams-configs`
- B. In Zookeeper under the `/kafka-streams` znode
- C. In the Kafka Streams application code itself
- D. In a separate config file read by the Kafka Streams application

**Answer:** C

**Explanation:**
In a Kafka Streams application, the processing topology (the DAG of processing nodes) is defined in the application code itself using the Kafka Streams DSL or the Processor API.

- A, B are incorrect because Kafka Streams does not use a special topic or Zookeeper for storing topology configurations.
- D is incorrect as the topology is not defined in a separate config file, but rather in the application code.

## Question 12

You are implementing a Kafka Streams application. The input is a KStream from a topic where the message values are in Avro format. What should you set for the `default.value.serde` property in the Streams configuration?

- A. `Serdes.String()`
- B. `Serdes.ByteArray()`
- C. `SpecificAvroSerde`
- D. `GenericAvroSerde`

**Answer:** C

**Explanation:**
In a Kafka Streams application, you need to specify the default serdes (serializers/deserializers) for message keys and values. Since the input topic has message values in Avro format, you should set:

- `default.value.serde=SpecificAvroSerde`: This tells Kafka Streams to use the `SpecificAvroSerde` to deserialize the message values. This assumes that you have specific Avro-generated classes for your data schema.

The other options are not ideal:

- A: `Serdes.String()` would be incorrect because the message values are in Avro format, not plain strings.
- B: `Serdes.ByteArray()` could work, but it would give you raw bytes that you'd have to deserialize manually. It's better to use a specific Avro serde.
- D: `GenericAvroSerde` could be used if you don't have specific Avro-generated classes and want to use the generic Avro record representation. But if you have specific classes, `SpecificAvroSerde` is preferred.

## Question 13

What is the recommended way to enhance the performance of a Kafka Streams application that does a simple map transformation on the input data?

- A. Increase the number of partitions of the output topic
- B. Enable state store caching
- C. Increase the commit interval
- D. Disable logging

**Answer:** A

**Explanation:**
In a Kafka Streams application that performs a simple stateless transformation like `map`, the bottleneck is often in the processing of the output topic. Increasing the number of partitions of the output topic allows more consumer instances to read from the topic in parallel, thereby increasing the overall throughput.

- B is not applicable because state store caching is useful for stateful operations, not for stateless transformations like `map`.
- C is incorrect because increasing the commit interval can actually decrease performance by causing larger batches to accumulate before being processed.
- D is incorrect because disabling logging does not directly enhance performance and can make debugging more difficult.

## Question 14

You are running a Kafka Streams application in a Docker container. The application performs a complex join operation and maintains a large state store. Which of the following would provide the greatest performance improvement when restarting the container?

- A. Increase the heap size of the Docker container
- B. Mount a high-performance SSD for the RocksDB directory
- C. Increase the number of replicas for the input topics
- D. Use a more powerful CPU for the Docker host

**Answer:** B

**Explanation:**
In a Kafka Streams application that maintains a large state store (e.g., for a complex join operation), the bottleneck during restarts is often the time taken to restore the state store from the changelog topic. By mounting a high-performance SSD for the RocksDB directory used by Kafka Streams, the state restore process can be significantly speeded up.

- A is less effective because the heap is used for processing, but the state store is stored on disk (in RocksDB) and loaded into off-heap memory.
- C does not directly impact the state restore performance because the changelogs are already replicated.
- D can help with processing speed but does not address the state restore bottleneck.

## Question 15

You are deploying a Kafka Streams application that joins two high-volume streams. Which of the following is LEAST likely to improve the performance of the application?

- A. Ensuring that the two input streams have the same number of partitions
- B. Increasing the number of standby replicas for the state store
- C. Tuning the `cache.max.bytes.buffering` parameter
- D. Increasing the `num.streams.threads` parameter

**Answer:** B

**Explanation:**
Increasing the number of standby replicas for the state store in a Kafka Streams application can provide better fault tolerance by allowing faster failover to a replica if a node fails. However, it is unlikely to improve the normal operating performance of the application.

In contrast:

- A can improve performance by allowing the join to be performed more efficiently, with each partition able to be processed independently.
- C can improve performance by allowing more data to be buffered in memory before being flushed to the state store, reducing I/O overhead.
- D can improve performance by allowing more partitions to be processed concurrently, up to the number of partitions of the input topics.

## Question 16

Which of the following stream processing operations can be considered stateful? (Select all that apply)

- A. Filter: Discarding messages based on a condition
- B. Map: Transforming messages from one format to another
- C. Aggregate: Combining multiple messages into a single result
- D. Join: Combining messages from two different streams based on a common key
- E. Peek: Performing an action on each message without modifying it

**Explanation:**
In stream processing, stateful operations are those that maintain and update a state based on the processed messages. They require the stream processor to keep track of some information over time.

1. Aggregate: Aggregation operations, such as counting, summing, or averaging values, are stateful because they need to maintain a running state of the aggregated result. The state is updated as new messages arrive, and the final result depends on the accumulated state over time.

2. Join: Join operations, especially window-based joins, are stateful because they need to maintain a state of the messages from both streams within a specified time window. The stream processor must store the messages temporarily to match and combine them based on a common key.

The other operations mentioned are generally considered stateless:

- Filter: Filtering messages based on a condition does not require maintaining any state. Each message is evaluated independently against the condition, and the decision to keep or discard the message is made solely based on the current message.

- Map: Transforming messages from one format to another is typically a stateless operation. Each message is processed independently, and the transformation logic is applied to each message without relying on any previous state.

- Peek: Performing an action on each message without modifying it, such as logging or publishing metrics, is usually stateless. The action is performed on each message independently, without maintaining any state across messages.

It's important to note that the specific implementation and requirements of a stream processing application can introduce statefulness to operations that are typically considered stateless. However, in general, aggregation and join operations are inherently stateful, while filtering, mapping, and peeking are often stateless.

## Question 17

What is the purpose of state stores in Kafka Streams?

- A. To persist the intermediate results and enable fault tolerance
- B. To cache the input messages for faster processing
- C. To store the final output of the stream processing application
- D. To maintain the configuration of the Kafka Streams application

**Answer:** A

**Explanation:**
In Kafka Streams, state stores play a crucial role in persisting the intermediate results and enabling fault tolerance for stateful operations.

When a Kafka Streams application performs stateful operations, such as aggregations or joins, it needs to maintain and update a state based on the processed messages. State stores provide a way to persistently store this state outside of the streaming application's memory.

The purpose of state stores is as follows:

1. Persistence: State stores allow the streaming application to persist the intermediate state to disk. This ensures that the state is not lost if the application fails or needs to be restarted. When the application restarts, it can reload the state from the state stores and resume processing from where it left off.

2. Fault Tolerance: By persisting the state, state stores enable fault tolerance in Kafka Streams applications. If a node in the Kafka Streams cluster fails, another node can take over the processing and recover the state from the state stores. This ensures that the processing can continue without losing the accumulated state.

3. Queryable State: State stores in Kafka Streams also provide the ability to query the current state of the application. This allows other applications or services to retrieve the latest computed state without needing to process the entire stream again. Queryable state is useful for serving real-time results or building interactive applications.

State stores in Kafka Streams are backed by an embedded key-value store, such as RocksDB, which provides efficient storage and retrieval of state data. Kafka Streams takes care of managing the state stores, including their creation, updates, and fault tolerance, based on the defined topology and configuration.

It's important to note that state stores are not used for caching input messages or storing the final output of the stream processing application. They are specifically designed to persist and manage the intermediate state required for stateful operations.

**Related Area:** Kafka Streams

## Question 18

How does Kafka Streams handle state recovery in case of a failure?

- A. By replaying all the input messages from the beginning
- B. By restoring the state from a snapshot stored in Kafka
- C. By rebuilding the state from the change log topic
- D. By retrieving the state from an external database

**Answer:** C

**Explanation:**
Kafka Streams provides built-in fault tolerance and state recovery mechanisms to handle failures and ensure the integrity of the processing state. When a failure occurs, Kafka Streams automatically recovers the state using the change log topic.

Here's how state recovery works in Kafka Streams:

1. Change Log Topic: For each state store in a Kafka Streams application, Kafka Streams creates a corresponding change log topic. The change log topic acts as a persistent log of all the state changes that occurred in the state store.

2. State Updates: Whenever the state in a state store is updated as a result of processing messages, Kafka Streams writes the state changes to the change log topic. Each record in the change log topic represents a state update and includes the key, value, and timestamp of the update.

3. Failure Recovery: If a failure occurs and a Kafka Streams application needs to recover its state, it starts by reading the change log topic from the beginning. The application replays the state changes from the change log topic to rebuild the state store. By replaying the state changes in the correct order, the application can restore its state to the latest consistent point before the failure.

4. Resuming Processing: Once the state is recovered from the change log topic, the Kafka Streams application can resume processing from the point where it left off. It continues to read input messages from the source topics and applies the processing logic to update the state and generate output.

- B. leveraging the change log topic, Kafka Streams ensures that the state can be recovered accurately and efficiently in case of failures. The change log topic acts as a durable and replicated log of state changes, providing a reliable source of truth for state recovery.

It's important to note that Kafka Streams does not rely on replaying all the input messages from the beginning or storing snapshots of the state in Kafka itself. The change log topic is specifically designed to capture and persist the state changes, enabling quick and precise state recovery.

Additionally, Kafka Streams does not rely on external databases for state storage or recovery. The state is managed internally within Kafka Streams using the embedded key-value stores and the change log topics.

## Question 19

You have a Kafka Streams application that processes messages from an input topic with 6 partitions. The application performs a stateful aggregation using a KTable. How many local state stores will be created by default?

- A. 1
- B. 3
- C. 6
- D. 12

**Answer:** C

**Explanation:**
In a Kafka Streams application, when you perform a stateful operation such as aggregation using a KTable, Kafka Streams creates local state stores to maintain the aggregated state for each partition of the input topic. By default, Kafka Streams creates one local state store per partition.

In this scenario, with an input topic having 6 partitions, the Kafka Streams application will create 6 local state stores by default, one for each partition.

Each local state store is associated with a specific partition and maintains the aggregated state for that partition. When a message is processed from a particular partition, the corresponding local state store is updated accordingly.

The number of local state stores created by Kafka Streams is determined by the number of partitions in the input topic and the parallelism of the Kafka Streams application. By default, Kafka Streams uses a parallelism of 1, which means it creates one stream task per partition. Each stream task is responsible for processing messages from its assigned partition and updating the corresponding local state store.

If you increase the parallelism of the Kafka Streams application, multiple stream tasks can be created to process messages from the same partition. In that case, the local state stores are shared among the tasks processing the same partition to ensure consistency.

It's worth noting that the actual number of local state stores created may be influenced by factors such as state store configuration, stream processing topology, and the specific Kafka Streams version being used.

## Question 20

What is the main advantage of using Kafka Streams DSL over the Processor API for stream processing?

- A. Kafka Streams DSL provides better performance compared to the Processor API
- B. Kafka Streams DSL offers a higher-level, declarative approach to defining stream processing logic
- C. Kafka Streams DSL supports stateful operations, while the Processor API is limited to stateless operations
- D. Kafka Streams DSL allows for easier integration with external systems compared to the Processor API

**Explanation:**
The main advantage of using Kafka Streams DSL (Domain-Specific Language) over the Processor API for stream processing is that it offers a higher-level, declarative approach to defining stream processing logic.

Kafka Streams DSL provides a set of high-level operations and constructs that allow developers to express stream processing logic in a more concise and expressive manner. With the DSL, you can chain together operations like `map`, `filter`, `groupBy`, `aggregate`, and `join` to define the desired stream processing topology. The DSL abstracts away low-level details and provides a more intuitive and readable way to define the processing logic.

On the other hand, the Processor API is a lower-level API that provides more fine-grained control over the stream processing topology. With the Processor API, you need to define individual processor nodes and connect them manually to create the desired processing flow. While this provides more flexibility, it requires more code and can be more complex to implement and maintain compared to the DSL.

Both Kafka Streams DSL and the Processor API offer similar performance characteristics and support stateful operations. They also provide integration capabilities with external systems. The choice between the two depends on the specific requirements of the application and the level of control and customization needed.

**Answer:** B