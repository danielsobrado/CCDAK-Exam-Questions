## Question 11

In a Kafka Streams application, where are the processing topology configurations stored?

A. In a special Kafka topic named `streams-configs`
B. In Zookeeper under the `/kafka-streams` znode
C. In the Kafka Streams application code itself
D. In a separate config file read by the Kafka Streams application

**Answer:** C

**Explanation:**
In a Kafka Streams application, the processing topology (the DAG of processing nodes) is defined in the application code itself using the Kafka Streams DSL or the Processor API.

- A, B are incorrect because Kafka Streams does not use a special topic or Zookeeper for storing topology configurations.
- D is incorrect as the topology is not defined in a separate config file, but rather in the application code.

## Question 12

You are implementing a Kafka Streams application. The input is a KStream from a topic where the message values are in Avro format. What should you set for the `default.value.serde` property in the Streams configuration?

A. `Serdes.String()`
B. `Serdes.ByteArray()`
C. `SpecificAvroSerde`
D. `GenericAvroSerde`

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

A. Increase the number of partitions of the output topic
B. Enable state store caching
C. Increase the commit interval
D. Disable logging

**Answer:** A

**Explanation:**
In a Kafka Streams application that performs a simple stateless transformation like `map`, the bottleneck is often in the processing of the output topic. Increasing the number of partitions of the output topic allows more consumer instances to read from the topic in parallel, thereby increasing the overall throughput.

- B is not applicable because state store caching is useful for stateful operations, not for stateless transformations like `map`.
- C is incorrect because increasing the commit interval can actually decrease performance by causing larger batches to accumulate before being processed.
- D is incorrect because disabling logging does not directly enhance performance and can make debugging more difficult.

## Question 14

You are running a Kafka Streams application in a Docker container. The application performs a complex join operation and maintains a large state store. Which of the following would provide the greatest performance improvement when restarting the container?

A. Increase the heap size of the Docker container
B. Mount a high-performance SSD for the RocksDB directory
C. Increase the number of replicas for the input topics
D. Use a more powerful CPU for the Docker host

**Answer:** B

**Explanation:**
In a Kafka Streams application that maintains a large state store (e.g., for a complex join operation), the bottleneck during restarts is often the time taken to restore the state store from the changelog topic. By mounting a high-performance SSD for the RocksDB directory used by Kafka Streams, the state restore process can be significantly speeded up.

- A is less effective because the heap is used for processing, but the state store is stored on disk (in RocksDB) and loaded into off-heap memory.
- C does not directly impact the state restore performance because the changelogs are already replicated.
- D can help with processing speed but does not address the state restore bottleneck.

## Question 15

You are deploying a Kafka Streams application that joins two high-volume streams. Which of the following is LEAST likely to improve the performance of the application?

A. Ensuring that the two input streams have the same number of partitions
B. Increasing the number of standby replicas for the state store
C. Tuning the `cache.max.bytes.buffering` parameter
D. Increasing the `num.streams.threads` parameter

**Answer:** B

**Explanation:**
Increasing the number of standby replicas for the state store in a Kafka Streams application can provide better fault tolerance by allowing faster failover to a replica if a node fails. However, it is unlikely to improve the normal operating performance of the application.

In contrast:

- A can improve performance by allowing the join to be performed more efficiently, with each partition able to be processed independently.
- C can improve performance by allowing more data to be buffered in memory before being flushed to the state store, reducing I/O overhead.
- D can improve performance by allowing more partitions to be processed concurrently, up to the number of partitions of the input topics.
