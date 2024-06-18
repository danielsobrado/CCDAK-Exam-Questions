## Kafka Consumer Overview

Kafka Consumers read records from Kafka topics. They can subscribe to one or more topics and process the stream of records produced to them. Consumers are part of consumer groups to ensure scalable and fault-tolerant processing.

### Points to Remember for CCDAK on Consumers

- **Consumer Groups**: Consumers within the same group divide topic partitions among themselves to ensure balanced processing.
- **Offset Management**: Consumers track their offset per partition to manage their position within the stream.
- **Rebalance Protocol**: Ensures partition ownership is balanced across all consumer instances in a group.
- **Fault Tolerance**: Consumers can recover from failures, continuing processing from their last committed offset.

When a consumer wants to join a group, it sends a JoinGroup request to the group coordinator. The first consumer to join becomes the group leader. The leader receives a list of all consumers in the group from the group coordinator (including those that sent a recent heartbeat and are considered alive) and is responsible for assigning a subset of partitions to each consumer.

### Partition Assignors

A `PartitionAssignor` is a class that, given consumers and the topics they subscribed to, decides which partitions will be assigned to which consumer. Kafka has two default assignment strategies:

#### 1. Range Assignor
* **How It Works**: Divides the sorted list of partitions into contiguous ranges and assigns each range to a consumer. The assignment is sequential based on the sorted order of consumer IDs.
* **Key Points**: Simple and predictable; however, it can lead to imbalanced workloads if the number of partitions is not a multiple of the number of consumers. It was the default partition assignor in Kafka versions prior to 2.4.
* **Example Scenario**: Imagine a topic with 12 partitions (P0 to P11) and 3 consumers (C0, C1, C2). The assignment might look like this: C0: P0, P1, P2, P3   -   C1: P4, P5, P6, P7  -  C2: P8, P9, P10, P11

#### 2. Round Robin Assignor
* **How It Works**: Assigns partitions to consumers in a round-robin fashion across all consumers, ensuring each consumer gets a partition before any consumer gets a second one.
* **Key Points**: Provides better load balancing compared to the Range Assignor, especially when the number of partitions is not evenly divisible by the number of consumers. However, it can lead to rebalancing when consumers are added or removed.
* **Example Scenario**: Using the same topic with 12 partitions (P0 to P11) and 3 consumers (C0, C1, C2), the assignment would be: C0: P0, P3, P6, P9   -   C1: P1, P4, P7, P10   -   C2: P2, P5, P8, P11

###### 3. Sticky Assignor
* **How It Works**: Aims to maintain a sticky relationship between consumers and partitions across rebalances, minimizing the number of partitions that get reassigned to different consumers.
* **Key Points**: Reduces the amount of data that needs to be re-fetched when partitions are reassigned; offers a good balance between fairness and stability. It was introduced in Kafka 2.4 and became the default assignor, replacing the Range Assignor.
* **Example Scenario**: After an initial assignment, if a new consumer (C3) joins, the Sticky Assignor would try to minimize partition movement. It might reassign only a few partitions from the existing consumers to the new one, like this: C0: P0, P3, P6   -   C1: P1, P4, P7   -   C2: P2, P5, P11   -   C3: P8, P9, P10

#### 4. Cooperative Sticky Assignor
* **How It Works**: Similar to the Sticky Assignor but allows for more cooperative rebalancing. It minimizes the impact on consumers by only reassigning partitions that need to be moved instead of revoking all partitions and redistributing them.
* **Key Points**: Designed to reduce the impact of rebalancing, allowing consumers to continue consuming while rebalance is in progress, and reducing the time it takes to complete a rebalance. It was introduced in Kafka 2.4 as an improvement over the Sticky Assignor.
* **Example Scenario**: When a consumer leaves the group, the Cooperative Sticky Assignor would only reassign the partitions that were consumed by the leaving consumer, without affecting the assignments of other consumers. This minimizes the rebalancing impact on the consumer group.

#### Mnemonic to Remember
**"RRSC" - Round, Range, Sticky, Cooperative**
* **Round** for **Round Robin**: Think of a round table where everyone gets a slice of cake one by one.
* **Range** for **Range Assignor**: Imagine dividing a chocolate bar into contiguous pieces where each person gets a range of pieces.
* **Sticky** for **Sticky Assignor**: Like sticky notes, once a partition is assigned to a consumer, it tries to "stick" with them across rebalances.
* **Cooperative** for **Cooperative Sticky Assignor**: Think of a team project where everyone cooperates, making changes only when necessary, thus minimizing disruption.

### Important Consumer Properties

#### `group.initial.rebalance.delay.ms`
- **Default**: 3000 (3 seconds)
- **Description**: Delay time before initial rebalance. A higher value can reduce rebalance storms in large clusters.
- **Trade-offs**: Longer delays may slow initial startup time for consumer groups.

#### `max.poll.intervals.ms`
- **Default**: 300000 (5 minutes)
- **Description**: Maximum delay between invocations of poll() when using consumer group management. Exceeding this will cause the consumer to leave the group.
- **Trade-offs**: Higher values provide more leeway for processing, but risk slower recovery from failures.

#### `max.poll.records`
- **Default**: 500
- **Description**: Maximum number of records returned in a single call to poll().
- **Trade-offs**: Lower values improve latency at the cost of throughput, and vice versa.

#### `enable.auto.commits`
- **Default**: true
- **Description**: If true, offsets are committed automatically at intervals.
- **Trade-offs**: Manual offset control can improve accuracy at the cost of convenience.

#### `auto.offset.reset`
- **Default**: latest
- **Description**: Controls the action when no initial offset is found for a consumer's group. Options are "earliest", "latest", or "none".
- **Trade-offs**: "earliest" may result in more data being processed, "latest" might miss messages if the consumer is down.

#### `fetch.min.bytes`
- **Default**: 1
- **Description**: Minimum data the server should return for a fetch request. Helps in controlling the number of updates.
- **Trade-offs**: Larger values can improve throughput but increase latency.

#### `max.partition.fetch.bytes`
- **Default**: 1048576 (1MB)
- **Description**: Maximum amount of data per partition the server will return.
- **Trade-offs**: Larger values increase throughput but can lead to more memory use.

#### `fetch.max.bytes`
- **Default**: 52428800 (50MB)
- **Description**: Maximum amount of data the server will return for a fetch request across all partitions.
- **Trade-offs**: Higher values allow more data to be fetched in a single request, affecting memory usage.

#### `session.timeout.ms`
- **Default**: 10000 (10 seconds)
- **Description**: Time used to detect consumer failures. If no heartbeat is received within this time, the consumer is considered dead.
- **Trade-offs**: Lower values make detection faster but may cause unnecessary rebalances.

#### `heartbeat.interval.ms`
- **Default**: 3000 (3 seconds)
- **Description**: Expected time between heartbeats to the group coordinator.
- **Trade-offs**: Lower values may lead to more frequent rebalances.

#### `isolation.level`
- **Default**: read_uncommitted
- **Description**: Controls visibility of transactional messages. "read_committed" filters out transactions not yet committed.
- **Trade-offs**: "read_committed" ensures cleaner data at the cost of potential latency.

#### `partition.assignment.strategy`
- **Default**: [org.apache.kafka.clients.consumer.RangeAssignor]
- **Description**: Determines the strategy for partition assignment among consumers.
- **Trade-offs**: Different strategies can optimize for fairness or throughput.

#### `client.rack`
- **Default**: Not set
- **Description**: Specifies the client's rack to enable rack-aware partition assignment.
- **Trade-offs**: Can reduce cross-rack traffic at the cost of potential imbalance in local traffic.

### Additional Consumer Configurations and Practices

#### Understanding Consumer Behavior

- **Subscription Types**: Kafka consumers can subscribe to topics in different ways, such as by specific topic names, pattern matching on topic names, or direct partition assignment. (e.g. `consumer.subscribe(Pattern.compile(".*topic"));` and `consumer.subscribe(Arrays.asList("orders-topic", "customers-topic", "sales-topic", "stocks-topic"));`)
- **Polling Process**: Inside the consumer's `poll()` call, several checks and operations occur, including heartbeat checks, subscription updates, and potentially triggering rebalance if necessary.

#### Consumer Rebalancing Triggers

Rebalancing, a critical aspect of Kafka consumer groups for ensuring even data processing distribution, can be triggered by:
- Changes in the consumer group (e.g., new consumer joining, existing consumer leaving).
- Changes in topic partition counts.
- Manual rebalance triggers like calling `unsubscribe()`.

 Consumer group rebalances are managed by the Kafka consumers themselves, not by the Controller. When a consumer joins or leaves a consumer group, the consumers coordinate among themselves to redistribute the partitions they are consuming from.

#### Managing Consumer Offsets

- **Auto-Commit Strategy**: By default, Kafka consumers automatically commit offsets at regular intervals, but manual offset management can provide finer control over when and how offsets are committed, which is essential for precise processing records management.
- **Offset Reset Behavior**: Determines how consumers behave when no initial offset is found. `auto.offset.reset` can significantly impact consumer startup behavior, especially in scenarios where precise data replay or skipping is required.
- The consumer is responsible for committing the offsets to the `__consumer_offsets` topic. However, **the consumer does not directly write the offset information** to the topic. Instead, it sends the offset information to the **group coordinator broker**, which then writes the offsets to the `__consumer_offsets` topic on behalf of the consumer.

#### Multi-threading and Consumer Safety

- **Single-threaded Nature**: Each Kafka consumer instance is not thread-safe. The recommended practice is one consumer per thread to avoid concurrency issues.
- **Consumer Thread Safety**: While the consumer itself is not thread-safe, certain operations like `wakeup()` can be safely called from another thread to interrupt an ongoing operation, such as a long `poll()`.  (`WakeupException` will be thrown)

#### High Availability and Fault Tolerance

- **Consumer Group Coordination**: Kafka uses the GroupCoordinator and ConsumerCoordinator components to manage consumer groups' state and rebalancing, ensuring consumers within a group efficiently distribute workload among themselves.
- **Heartbeats and Session Management**: By adjusting `heartbeat.interval.ms` and `session.timeout.ms`, you can fine-tune how quickly Kafka detects failed consumers and rebalances partitions, balancing between responsiveness and stability.

#### Consumer Polling and Processing Patterns

- **Long Processing Times**: If your consumer processing time might exceed `max.poll.interval.ms`, consider batching records and processing them more efficiently or adjusting the `max.poll.interval.ms` to allow more time for processing while avoiding unintended group rebalancing.
- **Batch Processing**: Consumers can adjust `max.poll.records` to control the number of records fetched in each poll call, allowing for more predictable processing times and better throughput control.

#### Offset and Partition Management

- **Manual Partition Assignment**: Using the `assign()` method allows for direct control over which partitions a consumer processes, bypassing Kafka's consumer group management and rebalance protocol. This approach is suitable for scenarios requiring static partition assignments.
- **Custom Offset Storage**: While Kafka provides built-in offset management through the `__consumer_offsets` topic, consumers can implement custom offset storage mechanisms if needed, allowing for greater flexibility in offset management strategies.
- Kafka allows specifying the position using `seek(TopicPartition, long)` to specify the new position (offset) a consumer can read from. Also `seekToBeginning(TopicPartition tp)` and `seekToEnd(TopicPartition tp)`.

### Optimizing Consumer Performance

- **Tuning Fetch Parameters**: Adjust `fetch.min.bytes`, `fetch.max.bytes`, and `max.partition.fetch.bytes` to optimize the balance between latency and throughput based on your application's specific needs.
- **Consumer Lag Monitoring**: Keeping an eye on `records-lag-max` can help identify when a consumer is not keeping up with the producers, allowing for timely adjustments to consumer configurations or scaling out consumers.

### High Watermark in Kafka

The **high watermark (HW)** is a critical concept in Kafka, ensuring data consistency and reliability. It represents the offset of the last message that has been successfully replicated to all **In-Sync Replicas (ISR)** of a partition.

### Group Coordinator and Group Leader

The **group coordinator** is a designated **broker** that receives heartbeats from all `consumers` within a `consumer group`. Each `consumer group` is assigned a single group coordinator. If a consumer fails to send heartbeats, the coordinator initiates a rebalance process.

When a consumer wishes to join a `consumer group`, it sends a `JoinGroup` request to the group coordinator. The first consumer to join the group is appointed as the group leader.

### Metadata request

When a consumer has the wrong partition leader, it sends a metadata request to any Kafka broker. The response includes metadata for each partition, grouped by topic:

- **Leader**: The current leader broker's node ID for the partition (-1 if no leader exists).
- **Replicas**: The alive slave brokers for the partition's leader.
- **ISR**: The subset of caught-up replicas.
- **Broker**: The node ID, hostname, and port of a Kafka broker.

#### Key Points:

1. **Definition**:
   - The high watermark is the offset of the last message replicated to all ISR.
   - It ensures that consumers only read fully replicated messages.

2. **In-Sync Replicas (ISR)**:
   - ISR are replicas that are fully caught up with the leader replica.
   - The leader handles all reads and writes for a partition.

3. **Replication and Acknowledgements**:
   - When a producer sends a message, it is written to the leader replica and then replicated to all ISR.
   - Producers specify the acknowledgment level (`acks`):
     - `acks=0`: No acknowledgment needed.
     - `acks=1`: Leader acknowledgment only (default).
     - `acks=-1` or `acks=all`: All ISR acknowledgment.

4. **Message Visibility**:
   - Consumers can only read messages up to the high watermark.
   - This ensures that only fully replicated messages are read, maintaining data consistency.
   - If `acks=1`, the highest offset (latest message) can be greater than the high watermark if the message is not yet replicated.

#### Example Scenario:

- A producer sends a message with offset 10.
- With `acks=1`, the leader acknowledges the write before replication.
- The high watermark may remain at offset 9 until the message at offset 10 is replicated to all ISR.
- Consumers will read messages up to offset 9 until offset 10 is fully replicated.

### Security Considerations

- **Encryption and Authentication**: Secure consumer connections to Kafka brokers using SSL/TLS for encryption and SASL for authentication to protect data in transit and ensure that only authorized consumers can access topic data.
