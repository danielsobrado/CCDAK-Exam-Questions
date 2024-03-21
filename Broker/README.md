## Kafka Broker Overview

 A Kafka Broker is a term for a server in a Kafka cluster that hosts topics and processes clients’ read and write requests. Brokers are responsible for preserving published data for a certain period or size. The Kafka broker makes the system scalable and fault-tolerant system.

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

#### `log.cleaner.backoff.ms`, `log.cleanup.policy`
- **Defaults**: 15000 (15 seconds), "delete"
- **Description**: Control the log cleaner's behavior and backoff time for log cleaning.
- **Trade-offs**: Adjusting these can affect log compaction and deletion performance.

#### `message.format.version`, `message.max.bytes`
- **Defaults**: Latest Kafka version, 1000012 bytes
- **Description**: Control the message format version and maximum size of a message.
- **Trade-offs**: Larger messages can impact performance and broker stability.

#### `num.io.threads`, `num.network.threads`, `num.recovery.threads.per.data.dir`
- **Defaults**: Dependent on runtime, 3, 1
- **Description**: Configure the number of threads for IO, network operations, and log recovery.
- **Trade-offs**: Increasing threads can improve performance but requires adequate hardware resources.

#### `offset.retention.minutes`
- **Default**: 10080 (7 days)
- **Description**: Time to retain offsets for a consumer group.
- **Trade-offs**: Longer retention may increase Zookeeper storage requirements.

#### `zookeeper.connect`, `zookeeper.connection.timeout.ms`
- **Default**: Depends on setup, 6000 ms
- **Description**: Zookeeper connection details and timeout settings.
- **Trade-offs**: Critical for broker coordination, mis
