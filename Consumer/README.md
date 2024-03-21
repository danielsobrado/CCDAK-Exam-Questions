## Kafka Consumer Overview

Kafka Consumers read records from Kafka topics. They can subscribe to one or more topics and process the stream of records produced to them. Consumers are part of consumer groups to ensure scalable and fault-tolerant processing.

### Points to Remember for CCDAK on Consumers

- **Consumer Groups**: Consumers within the same group divide topic partitions among themselves to ensure balanced processing.
- **Offset Management**: Consumers track their offset per partition to manage their position within the stream.
- **Rebalance Protocol**: Ensures partition ownership is balanced across all consumer instances in a group.
- **Fault Tolerance**: Consumers can recover from failures, continuing processing from their last committed offset.

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
