## Question 1:
In a Kafka cluster, two topics, `TopicA` and `TopicB`, are configured to be co-partitioned. This means they have an identical number of partitions, and messages in corresponding partitions are related to each other. Given the following configurations for these topics:

- `TopicA` has 6 partitions.
- `TopicB` is configured with the same number of partitions as `TopicA`.
- Both topics are consumed by a single consumer group with the `max.poll.records` configuration set to 500.

Assuming that the consumer processes messages from both topics in a manner that maintains their relationship, what is a critical consideration for ensuring data consistency across these co-partitioned topics?

1. Ensuring both topics have the same `replication.factor` to prevent data loss.
2. Using a custom partitioner that assigns messages to the same partition number in both topics based on key.
3. Configuring `TopicB` with a higher number of partitions than `TopicA` to ensure scalability.
4. Increasing `max.poll.records` to a higher value to ensure more messages are processed in each poll.

Choose the correct answer.

**Response:**

The correct answer is **2. Using a custom partitioner that assigns messages to the same partition number in both topics based on key.**

## Question 2:
For a streaming application processing data from two co-partitioned topics, `TopicX` and `TopicY`, which configuration ensures that the stream processing application maintains message ordering and correlation between these topics?

1. Configuring both topics with `log.compaction=true` to ensure message deduplication.
2. Ensuring that both topics are consumed by the application in separate threads.
3. Assigning a unique consumer group for each topic to maximize parallel processing.
4. Ensuring both topics use the same key for related messages and are consumed by the same consumer group.

Choose the correct answer.

**Response:**

The correct answer is **4. Ensuring both topics use the same key for related messages and are consumed by the same consumer group.**

## Question 3:
In a Kafka Streams application that enriches user clickstream data by joining it with user profile information, what should be the characteristics of the topic storing user profiles for optimal join operations?

```java
StreamsBuilder builder = new StreamsBuilder();
KStream<String, String> clicks = builder.stream("clickstream-topic");
KTable<String, String> profiles = builder.table("user-profiles-topic");
KStream<String, String> enrichedClicks = clicks.join(profiles,
    (click, profile) -> click + " enriched with " + profile);
enrichedClicks.to("enriched-clickstream-topic");
builder.build();
```

Choose the best topic configuration for `user-profiles-topic`.

1. compression.type = snappy
2. cleanup.policy = delete
3. cleanup.policy = compact

**Response:**

The correct answer is **3. cleanup.policy = compact.**

**Explanation:**
The `user-profiles-topic`, being used as a `KTable`, represents a changelog of user profile information. Using a `cleanup.policy` of `compact` ensures that the topic retains only the latest state of each user profile, which is essential for performing accurate and efficient joins with the clickstream data.

## Question 4:
When developing a Kafka Streams application that aggregates transaction amounts by user ID from a financial transactions topic, which key-serde configuration ensures optimal processing and state store management?

```java
StreamsBuilder builder = new StreamsBuilder();
KStream<String, BigDecimal> transactions = builder.stream("transactions-topic",
    Consumed.with(Serdes.String(), Serdes.BigDecimal()));
KTable<String, BigDecimal> aggregatedTransactions = transactions
    .groupByKey()
    .reduce(BigDecimal::add, Materialized.as("aggregated-transactions-store"));
aggregatedTransactions.toStream().to("aggregated-transactions-topic");
builder.build();
```

Select the correct Serde configuration for both the input topic and the state store.

1. Key Serde = Serdes.String(), Value Serde = Serdes.BigDecimal()
2. Key Serde = Serdes.Long(), Value Serde = Serdes.Double()
3. Key Serde = Serdes.String(), Value Serde = Serdes.String()

**Response:**

The correct answer is **1. Key Serde = Serdes.String(), Value Serde = Serdes.BigDecimal().**

**Explanation:**
The application processes financial transactions where the key is presumably a user ID (as a String) and the value is a transaction amount (as a BigDecimal). Using `Serdes.String()` for the key and `Serdes.BigDecimal()` for the value ensures that the data is correctly serialized/deserialized for both Kafka topic interaction and state store management, facilitating efficient and accurate aggregations.

## Question 5:
In a Kafka Streams application designed to monitor and alert on abnormal application log levels, logs are streamed from a source topic. Each record contains the log level (e.g., ERROR, WARN, INFO) and the log message. The application filters for ERROR level logs and forwards them to an output topic for immediate action. Consider the following Kafka Streams code snippet:

```java
StreamsBuilder builder = new StreamsBuilder();
KStream<String, String> logs = builder.stream("application-logs");
KStream<String, String> errorLogs = logs.filter((key, value) -> value.contains("ERROR"));
errorLogs.to("error-logs-topic");
builder.build();
```

What is the most appropriate configuration for the `application-logs` source topic to optimize the performance of this Kafka Streams application?

1. retention.bytes = -1
2. cleanup.policy = compact
3. min.insync.replicas = 2

**Response:**

The correct answer is **1. retention.bytes = -1.**

**Explanation:**
Given that the application is monitoring and alerting on abnormal log levels, particularly focusing on ERROR logs, the source topic `application-logs` does not specifically benefit from log compaction (`cleanup.policy = compact`) or a higher replication guarantee (`min.insync.replicas = 2`) for performance. Setting `retention.bytes = -1` ensures that logs are not prematurely removed based on size, which is crucial for a monitoring application that may need to process a high volume of logs and should not miss any ERROR logs due to log rolling based on size constraints.

## Question 6:
For a Kafka Streams application that processes customer orders to calculate real-time metrics such as total orders per hour, considering the following code:

```java
StreamsBuilder builder = new StreamsBuilder();
KStream<String, Order> orders = builder.stream("orders-topic",
    Consumed.with(Serdes.String(), new JsonSerde<>(Order.class)));
KTable<Windowed<String>, Long> ordersPerHour = orders
    .groupByKey()
    .windowedBy(TimeWindows.of(Duration.ofHours(1)))
    .count(Materialized.as("orders-per-hour-metrics"));
ordersPerHour.toStream().to("orders-metrics-topic");
builder.build();
```

Which configuration ensures that the state store `orders-per-hour-metrics` is optimally managed for performance and recovery?

1. state.store.replication.factor = 3
2. state.store.log.compaction = true
3. commit.interval.ms = 100

**Response:**

The correct answer is **1. state.store.replication.factor = 3.**

**Explanation:**
For stateful operations such as windowed aggregation (`ordersPerHour`), ensuring that the state store (`orders-per-hour-metrics`) is replicated across multiple brokers is crucial for both performance and fault tolerance. Setting `state.store.replication.factor = 3` increases the resilience of the state store, allowing for faster recovery in the event of a broker failure and ensuring that real-time metrics calculations are less likely to be interrupted. While log compaction (`state.store.log.compaction = true`) is not a valid configuration for state stores, and reducing the commit interval (`commit.interval.ms = 100`) could improve throughput, it is the replication factor of the state store that most directly impacts its performance and reliability in a production environment.

## Question 7

Is Kafka Streams DSL ANSI SQL compliant?

- A. Yes
- B. No
- C. Partially
- D. It depends on the version

**Answer:** B

**Explanation:**
Kafka Streams DSL, which is used for writing stream processing applications, is not ANSI SQL compliant. It uses a fluent Java API that is inspired by SQL but does not aim for full compliance.

- A is incorrect as Kafka Streams DSL is not designed to be ANSI SQL compliant.
- C and D are incorrect because the non-compliance is not partial or version-dependent. It's a design choice.

## Question 8

What is the primary language used for writing Kafka Streams applications?

- A. Python
- B. Java
- C. Scala
- D. SQL

**Answer:** B

**Explanation:**
Kafka Streams is a Java library for building real-time, highly scalable, fault-tolerant, distributed applications for stream processing. The primary language for writing Kafka Streams applications is Java.

- A, C, D are incorrect because while Kafka Streams integrates with other JVM languages like Scala, and there are some Python wrappers available, the native and primary language is Java.

## Question 9

What is the role of RocksDB in Kafka Streams?

- A. It is used for storing output topics.
- B. It is used for storing intermediate processing state.
- C. It is used for storing the Kafka Streams application code.
- D. It is not used in Kafka Streams.

**Answer:** B

**Explanation:**
In Kafka Streams, RocksDB is used as the default local state store for storing intermediate processing state. This state represents the computed results of the stream processing that need to be maintained between processing cycles.

- A is incorrect because output topics are stored in Kafka, not RocksDB.
- C is incorrect as application code is not stored in RocksDB.
- D is incorrect because RocksDB is indeed used in Kafka Streams for state management.
