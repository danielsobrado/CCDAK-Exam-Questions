## Monitoring and Metrics in Kafka

Monitoring and Metrics in Kafka Kafka exposes rich metrics to monitor the health, performance, and utilization metrics of Kafka brokers, producers, and consumers. The ability to obtain these metrics quickly and at scale is vital for troubleshooting and optimizing Kafka cluster .

### Points to Remember for CCDAK on Monitoring and Metrics

- **JMX Integration**: Kafka metrics can be exposed via JMX, allowing integration with monitoring tools.
- **Custom Reporters**: Kafka supports pluggable metrics reporters for flexibility in how metrics are reported and consumed.
- **Performance Tuning**: Metrics are key to identifying bottlenecks and tuning Kafka for optimal performance.
- **Operational Insight**: Monitoring provides insights into Kafka's operational status, including throughput, latency, and resource utilization.

### Important Monitoring and Metrics Properties

#### `kafka.metrics.reporters`
- **Default**: ""
- **Description**: Comma-separated list of classes to report Kafka metrics. These reporters must implement the `kafka.metrics.KafkaMetricsReporter` interface.
- **Trade-offs**: Adding custom reporters can enhance monitoring capabilities but might introduce additional overhead.

#### `metric.reporters`
- **Default**: ""
- **Description**: Comma-separated list of classes to report metrics from producers or consumers. These reporters implement the `org.apache.kafka.common.metrics.MetricsReporter` interface.
- **Trade-offs**: Enables detailed monitoring of producer and consumer metrics, with the trade-off of potential performance impact due to monitoring overhead.

#### `metrics.num.samples`
- **Default**: 2
- **Description**: The number of samples maintained to compute metrics.
- **Trade-offs**: Higher values provide more accurate metrics but can consume more memory.

#### `metrics.sample.window.ms`
- **Default**: 30000 (30 seconds)
- **Description**: The time window associated with each sample. Affects how metrics are calculated over time.
- **Trade-offs**: Larger windows provide smoother metrics over time but may delay detection of spikes or drops in metric values.

#### `metrics.recording.level`
- **Default**: INFO
- **Description**: The highest recording level for metrics in clients. Options are INFO or DEBUG.
- **Trade-offs**: DEBUG provides more granular metrics but can significantly increase the amount of data collected, impacting performance and storage.

### Important Kafka Metrics to Monitor

- **UnderReplicatedPartitions:** Tracks partitions with ISR (in-sync replica) issues. Alert if greater than 0.
- **ActiveControllerCount:** Indicates the number of active controllers in the cluster. Should be 1. Alert if not 1.
- **OfflinePartitionsCount:** Monitors the number of partitions without an active leader. Should be 0. Alert if greater than 0.
- **RequestHandlerAvgIdlePercent:** Measures the average idle percentage of request handler threads. Low values may indicate resource contention.
- **NetworkProcessorAvgIdlePercent:** Tracks the average idle percentage of network processor threads. Low values suggest potential network bottlenecks.
- **IsrExpandsPerSec:** Monitors the rate at which replicas are added to the ISR list per second.
- **IsrShrinksPerSec:** Monitors the rate at which replicas are removed from the ISR list per second.
- **MessagesInPerSec:** Tracks the rate of incoming messages per second.
- **BytesInPerSec:** Measures the incoming data rate in bytes per second.
- **BytesOutPerSec:** Measures the outgoing data rate in bytes per second.
- **TotalTimeMs:** Tracks the total time taken for request processing. High values may indicate performance issues.
- **LogFlushRateAndTimeMs:** Monitors the rate and time of log flushes. High values can indicate I/O bottlenecks.
- **ConsumerLag:** Monitors the lag of consumers to track how many messages the consumer is behind.
- **FetchRequestsPerSec:** Tracks the number of fetch requests from consumers per second.
- **ProduceRequestsPerSec:** Tracks the number of produce requests from producers per second.
- **BrokerTopicMetrics:** Monitors topic-specific metrics such as bytes in/out, messages in/out, etc.
- **JvmMemoryUsage:** Tracks the memory usage of the JVM, helping to identify potential memory leaks or insufficient heap sizes.
- **GarbageCollectionTime:** Monitors the time spent in garbage collection. High values can indicate potential performance issues.
- **LeaderElectionRateAndTimeMs:** Monitors the rate and time taken for leader election. High values may indicate issues with the controller or network.
- **PartitionCount:** Tracks the total number of partitions across all topics in the cluster.
- **LeaderCount:** Measures the number of partitions for which a broker is the leader.
- **ZooKeeperRequestLatencyMs:** Monitors the latency of ZooKeeper requests made by Kafka.
- **ZooKeeperDisconnectsPerSec:** Tracks the rate of disconnects from ZooKeeper.
- **ControllerState:** Indicates whether a broker is currently the active controller (1 if active, 0 if not).
- **UnderMinIsrPartitionCount:** Tracks the number of partitions with insufficient in-sync replicas (ISR).
