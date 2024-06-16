## Kafka Topic Configurations

Kafka topics are categorized into partitions for scalability and replicated across brokers for fault tolerance. Topic configuration parameters play a critical role in determining the behavior of topics in terms of performance, durability, and availability.

### Key Points for CCDAK on Topics

- **Durability and Fault Tolerance**: Configurations like `replication.factor` determine how data is replicated to ensure availability in case of broker failures.
- **Scalability**: The `partition` count of a topic influences how data is distributed across the cluster and impacts parallel processing capabilities.
- **Data Consistency**: The `acks` setting affects how producers receive acknowledgments from brokers, impacting data consistency guarantees.
- **Order Guarantee:** Kafka guarantees that any consumer of a given topic-partition will always read that partition's events in exactly the same order as they were written.
  - If maintaining strict order within a partition is critical, set max.in.flight.requests.per.connection to 1. This ensures that while a message is retrying, no other messages can be sent that might overtake the retried message. (Unless `enable.idempotence` is enabled, as it prevents message reordering caused by retries.)
- **Partition-Specific Order:** Order is guaranteed only within a partition, not across partitions.
  - Repartitioning can disrupt the order of messages in Kafka, primarily because it changes the way records are assigned to partitions.
- **Variable Message Count:** Partitions do not need to have the same number of messages.
- **Partition-Specific Offsets:** Offsets only have meaning within a specific partition.
- **Data Retention:** Messages are kept only for a limited time in Kafka, with the default being one week.
- **Immutability:** Once data is written to a partition, it cannot be changed.
- **Partition Assignment:** Data is assigned randomly to a partition unless a key is provided.

After adding partitions later, it cannot be guaranteed that old messages will be on the same partition as new messages with the same key.

### Important Topic Properties

#### `acks`
- **Scope**: Producer
- **Description**: Determines the number of acknowledgments the producer requires from brokers before considering a request complete. Valid values are `0`, `1`, or `all`.
- **Impact**: This setting impacts data durability and producer throughput. Setting `acks=all` ensures higher data safety as it waits for all in-sync replicas to acknowledge. However, this may lower throughput compared to `acks=1` or `acks=0`, where the latter offers the highest throughput but with potential data loss.

#### `replication.factor`
- **Scope**: Topic
- **Description**: Specifies the number of copies (replicas) of a topic to maintain across the cluster.
- **Default Value**: This is a topic-level configuration set at the time of topic creation and doesn't have a universal default. It often defaults to the cluster's `default.replication.factor`.
- **Impact**: A higher replication factor increases data availability and fault tolerance but requires more disk space and network bandwidth. It is critical for ensuring that even if some brokers are down, your data remains accessible.

#### `partitions`
- **Scope**: Topic
- **Description**: Determines the number of partitions within a topic. Partitions are the basic unit of parallelism in Kafka, with each partition being independently consumed.
- **Default Value**: This is set at the time of topic creation and typically defaults to the cluster's `num.partitions` setting.
- **Impact**: More partitions allow greater parallel processing of data but can increase the overhead on the Kafka cluster and Zookeeper. Finding the right balance is key to optimizing performance and resource utilization.

### Essential Kafka Topic Configuration Parameters

#### Durability and Fault Tolerance

- **`replication.factor`**: Defines the number of replicated copies of a topic across the cluster, enhancing data availability and fault tolerance. The actual number can be set per topic and is crucial for maintaining data integrity in the event of broker failures.

#### Scalability Through Partitioning

- **`partitions`**: Controls the partition count for a topic, directly influencing data distribution and parallel processing capabilities across consumers. More partitions support higher parallelism but may increase cluster management overhead.

#### Data Consistency and Throughput

- **`acks`**: Configures acknowledgment requirements from brokers to producers, balancing between data consistency and throughput. Values range from `0` (fire and forget), `1` (leader acknowledgment), to `all` (full ISR acknowledgment).

### Advanced Topic Configurations for Fine-Tuning

#### Log Compaction and Retention

- **`cleanup.policy`**: Supports log compaction (`"compact"`) to retain at least the last known value for each key within a topic, crucial for stateful applications. 
  - The default `cleanup.policy` for Kafka topics is "delete". (Kafka will delete records that are older than the retention period specified by `retention.ms`.)
  - Use `kafka-topics.sh` CLI tool or Admin API to change the policy for existing topics.
  - The `delete` policy might also be used in tandem to prevent the state store from growing indefinitely, especially when windowed operations are involved. (Use `"compact,delete"` )
  - Kafka Streams often requires that the latest state be recoverable even after restarts or rebalancing, making compaction a necessity.
- **`min.cleanable.dirty.ratio`**: Determines the ratio of log segments eligible for compaction. Lower values trigger compaction sooner, helping maintain a cleaner log with less overhead.

#### Segment Management and Efficiency

- **`segment.ms`**: Configures the time Kafka waits before closing the current log segment and starting a new one. Segment management affects storage and can impact log compaction and retention behavior.

#### Timestamps and Ordering

- **`message.timestamp.type`**: Defines whether Kafka should use the message creation time (`CreateTime`) or the time of log append (`LogAppendTime`). This setting can impact message ordering and log retention policies.

### Key Considerations for Message Key and Size

#### Message Key Usage

- Utilizing a message key (`key!=null`) ensures that messages with the same key are always sent to the same partition, facilitating message ordering per key. However, modifying the partition count can disrupt this consistency.

#### Managing Message Sizes

- Kafka's default message size limit is 1MB. Exceeding this without proper configuration adjustments (`message.max.bytes`, `replica.fetch.max.bytes`, `fetch.message.max.bytes`) will result in a `MessageSizeTooLargeException`.
