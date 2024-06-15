## Kafka Broker Overview

A Kafka Broker is a term for a server in a Kafka cluster that hosts topics and processes clients’ read and write requests. Brokers are responsible for preserving published data for a certain period or size. The Kafka broker makes the system scalable and fault-tolerant.

### Points to Remember for CCDAK on Brokers

- **Scalability**: Brokers allow Kafka to scale out by adding more machines without downtime.
- **Fault Tolerance**: Replication across brokers ensures high availability and fault tolerance.
- **Configuration Management**: Broker configurations play a crucial role in Kafka performance and reliability.
- **Topic Management**: Brokers handle topic creation, partitioning, and replication.

### Important Broker Properties

#### `default.replication.factor`
- **Default**: 1
- **Description**: Default number of replicas for a topic. Increasing this can enhance fault tolerance.
- **Trade-offs**: Higher replication can lead to increased latency and resource usage.

#### `num.partitions`
- **Default**: 1
- **Description**: Default number of partitions per topic. More partitions allow higher parallelism.
- **Trade-offs**: Too many partitions can increase overhead on broker and Zookeeper performance.

#### `min.insync.replicas`
- **Default**: 1
- **Description**: Minimum number of replicas that must acknowledge a write for it to be considered successful.
- **Trade-offs**: A higher value increases data durability but may impact write latency.

#### `unclean.leader.election.enable`
- **Default**: true
- **Description**: Allows a non-synced replica to become leader. Turning it off ensures data consistency.
- **Trade-offs**: Disabling may increase availability but risks data loss.

#### `num.replication.fetchers`
- **Default**: 1
- **Description**: Number of fetcher threads used to replicate messages.
- **Trade-offs**: Increasing can improve replication throughput at the cost of higher CPU usage.

#### `replica.fetch.max.bytes`
- **Default**: 1048576 (1MB)
- **Description**: Maximum bytes a replica can fetch in a single request. Higher values can improve replication throughput.
- **Trade-offs**: Larger fetch sizes may increase memory usage.

#### `advertised.listeners`
- **Default**: Depends on setup
- **Description**: Hostname and port the broker will advertise to producers and consumers.
- **Trade-offs**: Must be set correctly for clients to connect.

#### `logs.dirs`
- **Default**: /tmp/kafka-logs
- **Description**: Directories where the log data is stored.
- **Trade-offs**: Adequate disk space and IO performance are critical.

#### `auto.create.topic.enable`
- **Default**: true
- **Description**: Allows automatic topic creation on the server.
- **Trade-offs**: Convenient but may lead to unintentional topic creation.

#### `log.retention.hours`, `log.retention.bytes`, `log.segment.bytes`
- **Defaults**: 168 hours (7 days), -1 (unlimited), 1073741824 (1GB)
- **Description**: Control the retention policy for logs by time, size, and segment size.
- **Trade-offs**: Balancing disk usage with data availability needs.

#### `log.retention.check.interval.ms`
- **Default**: 300000 (5 minutes)
- **Description**: Frequency at which the log cleaner checks for logs to clean.
- **Trade-offs**: More frequent checks can slightly increase CPU usage but help in timely log cleanup.

### Partition Management

- **Basics**: A Kafka topic consists of one or more partitions. This allows the data of a topic to be distributed across multiple brokers.
- **Replication Factor**: Each partition can have multiple replicas, with one being the leader. The replication factor determines the total number of these replicas.
- **Immutability and Ordering**: Once data is written to a partition, it is immutable. Kafka guarantees order within a partition but not across partitions.

### Segment Management

- **Partition Segments**: Partitions are subdivided into segments, which are essentially log files where Kafka messages are stored.
- **Segment Size and Duration**: Configurable settings such as `log.segment.bytes` and `log.segment.ms` control the size and time before a new segment is rolled out.
- **Indexes for Efficiency**: Each segment comes with index files to efficiently locate messages either by offset or timestamp.

### Log Retention and Cleanup

- **Policies**: Log segments are eligible for cleanup based on size or time, whichever is reached first. 
- **Cleanup Types**: Kafka supports deletion or compaction as log cleanup policies. Deletion simply removes old segments, while compaction retains at least the latest value for each key.

### Topic Replication

- **Leader and ISR**: For each partition, one replica is the leader that handles all read and write requests, while the others are in-sync replicas (ISR).
- **Replica Management**: Kafka manages replicas to ensure data is not lost and is accessible even if some brokers are down. The replication factor should not exceed the number of brokers in the cluster to ensure each partition has a unique set of brokers.

### Producer Considerations

- **Reliability**: Producers can specify `acks` to control the number of replicas that must acknowledge a write for it to be considered successful, balancing between reliability and performance.
- **Message Size**: The `message.max.bytes` setting controls the maximum size of a message that can be sent. Large messages can impact broker performance and stability.
- **Message Keys**: Producers can send messages with a key to ensure messages with the same key are sent to the same partition, enabling ordering by key within a partition.

### Consumer Considerations

- **Consumer Groups and Offset Management**: Consumers track their progress via offsets within each partition. Kafka stores these offsets in a special topic named `__consumer_offsets`.
- **Partition Assignment**: Consumers in a group are automatically assigned partitions by Kafka. This can be overridden with manual partition assignment if needed.

### Broker Performance Tuning

- **Thread Management**: Configuring the number of I/O and network threads (`num.io.threads`, `num.network.threads`) can significantly affect broker performance, especially in high-throughput environments.
- **Log Flush Management**: Settings like `log.flush.interval.messages` and `log.flush.interval.ms` control the frequency of log flushes to disk, impacting durability vs performance.

### Security Configurations

- **Encryption and Authentication**: Brokers can be configured to use SSL/TLS for encrypting client connections and SASL for client authentication, ensuring secure data transmission.
- **Authorization**: Kafka supports ACLs for authorizing client requests, allowing fine-grained control over who can read or write to topics.

### Monitoring and Management

- **JMX Metrics**: Kafka exposes a wide range of metrics via JMX, which can be used to monitor broker health, performance, and resource usage.
- **Log Clean-Up**: Monitoring log segment sizes and cleanup policies is crucial to avoid running out of disk space. Adjusting `log.retention.hours`, `log.retention.bytes`, and related settings helps manage disk usage.
- 
| Broker Metric | Explanation |
|---------------|-------------------|
| ACTIVE CONTROLLER COUNT | The broker with an Active Controller Count of 1 is the current controller of the Kafka cluster. The controller is responsible for managing the state of partitions and replicas, and electing leaders.|
| REQUEST HANDLER IDLE RATIO | This metric indicates the percentage of time the broker's request handlers are idle. A low idle ratio suggests the broker is under heavy load and may need additional resources or optimization.|
| ALL TOPICS BYTES IN | The All Topics Bytes In metric measures the incoming byte rate for all topics on the broker. High values indicate a need to scale out the Kafka cluster by adding more brokers. |
| ALL TOPICS BYTES OUT | This metric measures the outgoing byte rate for all topics on the broker, which reflects the consumer traffic. High consumer traffic may require increasing consumer instances or optimizing consumer configurations. |
| ALL TOPICS MESSAGES IN | The All Topics Messages In metric measures the incoming message rate for all topics on the broker.|
| PARTITION COUNT | This metric indicates the number of partitions assigned to a broker. Evenly distributing partitions across brokers is important for maintaining a balanced cluster. |
| LEADER COUNT | The Leader Count metric shows the number of partitions for which the broker is the leader. Leaders handle all read and write requests for partitions. |
| OFFLINE PARTITIONS | Offline partitions are partitions without an active leader. A high number of offline partitions can indicate issues with broker health or network connectivity.|
| REQUEST METRICS | Request metrics, such as request rate and request latency, provide insights into the performance of the broker and can help identify potential bottlenecks. |

### High Availability Considerations

- **Zookeeper Dependency**: Kafka brokers rely on Zookeeper for cluster metadata and coordination. Ensuring Zookeeper cluster's health is critical for Kafka's reliability.
- **Broker Failures**: Kafka's replication mechanism ensures that as long as a sufficient number of replicas are alive, brokers can fail without losing data. Properly configuring `min.insync.replicas` and `replication.factor` is key.

### Zero Copy

Most traditional data systems use RAM for data storage due to its low latencies.

Kafka avoids using RAM for this purpose, achieving low latency message delivery through the Zero Copy Principle. This optimization eliminates intermediate data copies, transferring data directly from the read buffer to the socket buffer.

However, enabling SSL for data encryption in transit negates zero copy optimization. With SSL, the broker must decrypt and re-encrypt data, thus reintroducing intermediate data handling.
