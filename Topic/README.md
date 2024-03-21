## Kafka Topic Configurations

Kafka topics are categorized into partitions for scalability and replicated across brokers for fault tolerance. Topic configuration parameters play a critical role in determining the behavior of topics in terms of performance, durability, and availability.

### Key Points for CCDAK on Topics

- **Durability and Fault Tolerance**: Configurations like `replication.factor` determine how data is replicated to ensure availability in case of broker failures.
- **Scalability**: The `partition` count of a topic influences how data is distributed across the cluster and impacts parallel processing capabilities.
- **Data Consistency**: The `acks` setting affects how producers receive acknowledgments from brokers, impacting data consistency guarantees.

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
