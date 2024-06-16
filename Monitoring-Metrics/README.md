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

### Broker Metrics:
- `UnderReplicatedPartitions`: Number of partitions that are under-replicated. Use case: Identifying replication issues. Alert if greater than 0.
- `ActiveControllerCount`: Number of active controllers in the cluster. Use case: Ensuring controller failover is working correctly. Should be 1. Alert if not 1. (The **unique** controller takes care of Leader Election, Cluster Metadata, Rebalancing Partitions and Handling Broker Failures)
- `OfflinePartitionsCount`: Number of partitions without an active leader. Use case: Detecting availability issues. Should be 0. Alert if greater than 0.
- `RequestHandlerAvgIdlePercent`: Average idle percentage of the request handler thread pool. Use case: Monitoring request handling capacity. Low values may indicate resource contention.
- `NetworkProcessorAvgIdlePercent`: Average idle percentage of the network thread pool. Use case: Monitoring network processing capacity. Low values suggest potential network bottlenecks.
- `IsrExpandsPerSec`: Rate at which the in-sync replica set is expanding. Use case: Tracking replica synchronization.
- `IsrShrinksPerSec`: Monitors the rate at which replicas are removed from the ISR list per second. Use case: Tracking replica synchronization issues.
- `BytesInPerSec` and `BytesOutPerSec`: Inbound and outbound byte rate of brokers. Use case: Monitoring data throughput.
- `MessagesInPerSec`: Rate at which messages are being produced to brokers. Use case: Tracking message production rate.
- `UnderMinIsrPartitionCount`: Number of partitions with insufficient in-sync replicas. Use case: Identifying replication issues.
- `PartitionCount`: Total number of partitions across all topics in the cluster. Use case: Monitoring cluster capacity.
- `LeaderCount`: Number of partitions for which a broker is the leader. Use case: Monitoring leadership distribution.
- `RequestQueueSize` and `ResponseQueueSize`: Size of request and response queues. Use case: Ensuring brokers can keep up with client requests.
- `ProduceTotalTimeMs` and `FetchTotalTimeMs`: Total time taken for produce and fetch requests. Use case: Monitoring request latency.
- `TotalTimeMs`: Tracks the total time taken for request processing. High values may indicate performance issues. Use case: Monitoring overall request latency.
- `FetchMessageConversionsPerSec`: Rate of message format conversions during fetch requests. Use case: Tracking message format compatibility.
- `ZooKeeperRequestLatencyMs`: Latency of ZooKeeper requests made by Kafka. Use case: Monitoring ZooKeeper performance.
- `ControllerState`: Indicates whether a broker is currently the active controller (1 if active, 0 if not). Use case: Monitoring controller health.
- `LogFlushRateAndTimeMs`: Monitors the rate and time of log flushes. High values can indicate I/O bottlenecks. Use case: Identifying disk performance issues.
- `LeaderElectionRateAndTimeMs`: Monitors the rate and time taken for leader election. High values may indicate issues with the controller or network. Use case: Tracking controller performance and stability.
- `JvmMemoryUsage`: Tracks the memory usage of the JVM, helping to identify potential memory leaks or insufficient heap sizes. Use case: Monitoring broker memory utilization.
- `GarbageCollectionTime`: Monitors the time spent in garbage collection. High values can indicate potential performance issues. Use case: Identifying JVM garbage collection overhead.
- `ZooKeeperDisconnectsPerSec`: Tracks the rate of disconnects from ZooKeeper. Use case: Identifying ZooKeeper connection issues.

### Consumer Metrics:
- `records-lag-max`: Maximum lag in terms of number of records for any partition in this window. Use case: Monitoring consumer lag.
- `fetch-latency-avg`: Average time taken for a fetch request. Use case: Monitoring consumer performance.
- `records-consumed-rate`: The average number of records consumed per second. Use case: Tracking consumer throughput.
- `FetchRequestsPerSec`: Tracks the number of fetch requests from consumers per second. Use case: Monitoring consumer activity.
- `ConsumerLag`: Monitors the lag of consumers to track how many messages the consumer is behind.

### Producer Metrics:
- `record-send-rate`: The average number of records sent per second. Use case: Monitoring producer throughput.
- `request-latency-avg`: The average request latency in ms. Use case: Monitoring producer latency.
- `outgoing-byte-rate`: The average number of outgoing bytes sent per second to all servers. Use case: Monitoring producer network utilization.
- `ProduceRequestsPerSec`: Tracks the number of produce requests from producers per second. Use case: Monitoring producer activity.

### Topic Metrics:
- `BrokerTopicMetrics`: Monitors topic-specific metrics such as bytes in/out, messages in/out, etc. Use case: Analyzing performance at the topic level.

### Confluent-Specific Metrics:
- Schema Registry: `schema-registry.registered.count`: Number of registered schemas. Use case: Monitoring schema usage.
- Kafka Connect: `connect-worker.connector-started-task-count`: Number of tasks started for a connector. Use case: Monitoring connector health.
- ksqlDB: `ksql-server.query-stats.bytes-consumed-total`: Total bytes consumed by ksqlDB queries. Use case: Monitoring ksqlDB resource usage.

### Important Kafka Metrics

#### `UnderReplicatedPartitions`
- **Description**: Represents the number of partitions that are under-replicated, meaning they have fewer in-sync replicas (ISRs) than the desired replication factor.
- **Importance**: This metric is crucial for monitoring the health and reliability of the Kafka cluster. A non-zero value indicates that some partitions are at risk of data loss if a broker fails.
- **Monitoring Recommendations**:
 - Alert if the value is consistently greater than 0.
 - Investigate the root cause of under-replication, such as broker failures, network issues, or insufficient disk space.
 - Ensure that the replication factor is set appropriately based on the desired fault tolerance.

#### `ActiveControllerCount`
- **Description**: Indicates the number of active controllers in the Kafka cluster. In a healthy cluster, there should be exactly one active controller.
- **Importance**: The controller is responsible for managing partition leader elections and maintaining the overall state of the cluster. Having multiple active controllers can lead to conflicts and instability.
- **Monitoring Recommendations**:
 - Alert if the value deviates from 1.
 - If multiple controllers are detected, investigate the root cause, such as network partitions or issues with ZooKeeper.

#### `OfflinePartitionsCount`
- **Description**: Represents the number of partitions without an active leader. These partitions are unavailable for producing or consuming messages.
- **Importance**: Offline partitions indicate a problem with the Kafka cluster and can impact data availability and processing.
- **Monitoring Recommendations**:
 - Alert if the value is greater than 0.
 - Investigate the reasons for offline partitions, such as broker failures, network issues, or insufficient replicas.

#### `BytesInPerSec` and `BytesOutPerSec`
- **Description**: Measures the inbound and outbound byte rate of Kafka brokers, indicating the amount of data being produced to and consumed from the cluster.
- **Importance**: Monitoring these metrics helps in understanding the data throughput and identifying potential bottlenecks or performance issues.
- **Monitoring Recommendations**:
 - Set appropriate thresholds based on the expected data volume and network capacity.
 - Monitor trends over time to detect anomalies or sudden spikes in data throughput.
 - Correlate with other metrics like `MessagesInPerSec` and `ProduceRequestsPerSec` to gain insights into producer and consumer behavior.

#### `ConsumerLag`
- **Description**: Represents the lag of consumers, indicating how many messages they are behind the latest offset in the partition they are consuming from.
- **Importance**: Consumer lag is a critical metric for monitoring the health and performance of Kafka consumers. High consumer lag can indicate issues like slow processing, insufficient consumer resources, or problems with the consumer application.
- **Monitoring Recommendations**:
 - Set appropriate lag thresholds based on the acceptable delay in message processing.
 - Alert if the lag exceeds the defined thresholds consistently.
 - Investigate the root cause of high consumer lag, such as slow processing logic, resource contention, or network issues.

#### `ProduceTotalTimeMs` and `FetchTotalTimeMs`
- **Description**: Measures the total time taken for produce and fetch requests, respectively. These metrics provide insights into the latency of producer and consumer operations.
- **Importance**: Monitoring these metrics helps in identifying performance bottlenecks and ensuring that produce and fetch operations are within acceptable latency ranges.
- **Monitoring Recommendations**:
 - Set appropriate latency thresholds based on the requirements of the application.
 - Alert if the latency consistently exceeds the defined thresholds.
 - Analyze trends over time to detect degradation in performance and investigate the underlying causes.