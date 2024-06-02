## Question 1
What is the purpose of the `process.roles` configuration in KRaft mode?

- A. To specify whether the server acts as a controller, broker, or both
- B. To set the unique identifier for the server
- C. To define the listeners used by the controller
- D. To configure the metrics reporter for the server

**Answer:** A

**Explanation:**
In KRaft mode, the `process.roles` configuration is used to specify whether the server acts as a controller, broker, or both. This configuration is mandatory for KRaft mode and determines the role of the server in the Kafka cluster.

- If set to `broker`, the server operates only as a broker.
- If set to `controller`, the server operates as a controller only.
- If set to `broker,controller`, the server operates in combined mode as both a broker and a controller. However, this mode is not supported for production use.

B, C, and D are incorrect because they do not describe the purpose of the `process.roles` configuration. The unique identifier is set using `node.id`, the listeners used by the controller are defined by `controller.listener.names`, and the metrics reporter is configured separately.

## Question 2
What is the recommended value for `process.roles` in a production KRaft cluster?

- A. `broker,controller`
- B. `broker` for broker nodes and `controller` for controller nodes
- C. `controller` for all nodes
- D. Leave `process.roles` unconfigured

**Answer:** B

**Explanation:**
In a production KRaft cluster, it is recommended to separate the roles of brokers and controllers. This means setting `process.roles` to `broker` for nodes that act as Kafka brokers and `controller` for nodes that act as controllers.

Separating the roles provides better isolation and allows for independent scaling of brokers and controllers based on the workload requirements. It also aligns with the best practices for deploying a resilient and scalable Kafka cluster.

A is incorrect because combined mode (`broker,controller`) is not supported for production use. It is only suitable for development and testing purposes.

C is incorrect because having all nodes as controllers would not leave any nodes to handle the actual data storage and message processing.

D is incorrect because leaving `process.roles` unconfigured would mean the cluster is not running in KRaft mode, and would default to the older ZooKeeper-based controller quorum.

## Question 3
What is the purpose of the `controller.quorum.voters` configuration in KRaft mode?

- A. To specify the listeners used by the controllers
- B. To set the minimum number of in-sync replicas for the controller quorum
- C. To define the list of voters in the controller quorum
- D. To configure the metrics reporter for the controllers

**Answer:** C

**Explanation:**
In KRaft mode, the `controller.quorum.voters` configuration is used to define the list of voters in the controller quorum. This configuration specifies the set of KRaft controllers that participate in the quorum for controller leader election and metadata management.

The `controller.quorum.voters` configuration is a comma-separated list of KRaft controller IDs, each in the format of `{id}@{host}:{port}`. For example:

controller.quorum.voters=1@controller1:9093,2@controller2:9093,3@controller3:9093

All the controllers specified in this list form the voting group for leader election and participate in the metadata replication process.

A is incorrect because the listeners used by the controllers are configured using the `controller.listener.names` property.

B is incorrect because the minimum number of in-sync replicas is not applicable to the controller quorum. The controller quorum operates based on a majority voting system.

D is incorrect because the metrics reporter is configured separately and is not related to the `controller.quorum.voters` configuration.

## Question 4
What is the minimum number of controllers required for a KRaft cluster?

- A. 1
- B. 2
- C. 3
- D. 4

**Answer:** C

**Explanation:**
In a KRaft cluster, the minimum number of controllers required is 3. This is because KRaft uses a majority voting system for leader election and metadata replication.

To achieve a quorum and maintain fault tolerance, the number of controllers in the cluster must be an odd number and at least 3. With 3 controllers, the cluster can tolerate the failure of a single controller while still maintaining a majority for decision making.

Having only 1 or 2 controllers (options A and B) would not provide fault tolerance, as the failure of a single controller would render the cluster unable to make progress.

Having 4 controllers (option D) is a valid configuration but is not the minimum required. The typical recommendation is to have 3 or 5 controllers in a KRaft cluster, depending on the desired level of fault tolerance.

## Question 5
What happens if a majority of the controllers in a KRaft cluster become unavailable?

- A. The cluster remains operational with reduced performance
- B. The cluster automatically elects a new set of controllers
- C. The cluster becomes unavailable until a majority of controllers are restored
- D. The brokers take over the responsibilities of the controllers

**Answer:** C

**Explanation:**
In a KRaft cluster, if a majority of the controllers become unavailable, the cluster becomes unavailable until a majority of controllers are restored. This is because KRaft relies on a quorum-based system for metadata management and leader election.

When a majority of controllers are unavailable, the remaining controllers do not have enough votes to form a quorum and make decisions. This means that no new leader can be elected, and no metadata changes can be processed. As a result, the cluster becomes unavailable, and no read or write operations can be performed.

To restore the cluster's availability, a majority of the controllers must be brought back online. Once a majority is available, the controllers can elect a leader and resume metadata processing, allowing the cluster to become operational again.

A is incorrect because the cluster does not remain operational with reduced performance. It becomes completely unavailable until a majority of controllers are restored.

B is incorrect because the cluster does not automatically elect a new set of controllers. The existing controllers must be restored to regain a majority.

D is incorrect because the brokers do not take over the responsibilities of the controllers. In KRaft mode, the controllers are separate from the brokers and have specific roles in metadata management.

## Question 6
What is the purpose of the `kafka-storage` tool in KRaft mode?

- A. To configure the storage directories for Kafka brokers
- B. To manage the Kafka consumer offsets
- C. To generate a cluster ID and format storage directories
- D. To monitor the disk usage of Kafka brokers

**Answer:** C

**Explanation:**
In KRaft mode, the `kafka-storage` tool is used to generate a cluster ID and format the storage directories for Kafka brokers and controllers. This is a necessary step before starting the Kafka cluster.

The `kafka-storage` tool provides two important commands:

1. `random-uuid`: Generates a new cluster ID for the Kafka cluster. For example:

   bin/kafka-storage random-uuid

2. `format`: Formats the storage directories for each broker and controller using the cluster ID. For example:

   bin/kafka-storage format -t <cluster-id> -c <path-to-config-file>

Formatting the storage directories initializes them with the necessary metadata and prepares them for use by the Kafka brokers and controllers.

A is incorrect because the `kafka-storage` tool does not configure the storage directories. It formats them using the cluster ID.

B is incorrect because managing consumer offsets is not the purpose of the `kafka-storage` tool. Consumer offsets are managed internally by Kafka.

D is incorrect because monitoring the disk usage of Kafka brokers is not the responsibility of the `kafka-storage` tool. There are separate monitoring tools and metrics for tracking disk usage.

## Question 7
What is the default location for the Kafka metadata log in KRaft mode?

- A. The first directory specified in the `log.dirs` configuration
- B. The directory specified in the `metadata.log.dir` configuration
- C. The directory specified in the `controller.log.dir` configuration
- D. The Kafka data directory

**Answer:** A

**Explanation:**
In KRaft mode, the default location for the Kafka metadata log is the first directory specified in the `log.dirs` configuration. If `log.dirs` contains multiple directories, the first directory in the list will be used to store the metadata log.

For example, if `log.dirs` is configured as follows:

log.dirs=/data/kafka-logs-1,/data/kafka-logs-2

The metadata log will be stored in the `/data/kafka-logs-1` directory by default.

To explicitly specify a different directory for the metadata log, you can use the `metadata.log.dir` configuration. If `metadata.log.dir` is set, it will override the default location derived from `log.dirs`.

B is incorrect because `metadata.log.dir` is not the default location. It is an optional configuration that overrides the default location.

C is incorrect because `controller.log.dir` is not a valid configuration in KRaft mode. The metadata log is not specific to controllers.

D is incorrect because there is no specific "Kafka data directory" in KRaft mode. The metadata log is stored in the directory specified by `log.dirs` or `metadata.log.dir`.

## Question 8
What is the purpose of the `kafka-metadata-quorum` tool in KRaft mode?

- A. To manage the Kafka consumer offsets
- B. To generate a cluster ID for the Kafka cluster
- C. To describe the runtime status of the KRaft metadata quorum
- D. To modify the Kafka topic configurations

**Answer:** C

**Explanation:**
In KRaft mode, the `kafka-metadata-quorum` tool is used to describe the runtime status of the KRaft metadata quorum. It provides information about the current state of the KRaft controllers and their metadata replication.

By running the `kafka-metadata-quorum` tool with the `describe` command and the `--status` flag, you can retrieve a summary of the metadata quorum status. For example:

bin/kafka-metadata-quorum --bootstrap-server localhost:9092 describe --status

The output of this command includes information such as:

- Cluster ID
- Leader ID and epoch
- High watermark and maximum follower lag
- Current voters and observers

This information is useful for monitoring the health and status of the KRaft metadata quorum and troubleshooting any issues related to the controllers.

A is incorrect because managing consumer offsets is not the purpose of the `kafka-metadata-quorum` tool. Consumer offsets are managed internally by Kafka.

B is incorrect because generating a cluster ID is not the responsibility of the `kafka-metadata-quorum` tool. The cluster ID is generated using the `kafka-storage` tool.

D is incorrect because modifying Kafka topic configurations is not the purpose of the `kafka-metadata-quorum` tool. Topic configurations can be modified using the `kafka-configs` tool.

## Question 9
Which of the following metrics is used to monitor the lag between the active KRaft controller and the last committed record in the metadata log?

- A. `kafka.controller:type=KafkaController,name=ActiveControllerCount`
- B. `kafka.controller:type=ControllerEventManager,name=EventQueueTimeMs`
- C. `kafka.controller:type=KafkaController,name=LastCommittedRecordOffset`
- D. `kafka.controller:type=KafkaController,name=LastAppliedRecordLagMs`

**Answer:** D

**Explanation:**
In KRaft mode, the metric `kafka.controller:type=KafkaController,name=LastAppliedRecordLagMs` is used to monitor the lag between the active KRaft controller and the last committed record in the metadata log.

This metric represents the difference between the local time and the append time of the last applied record batch. It indicates how far behind the active controller is in terms of applying the latest committed records from the metadata log.

- For active controllers, the value of `LastAppliedRecordLagMs` is always zero because they are up to date with the latest committed records.
- For inactive controllers, `LastAppliedRecordLagMs` measures the lag between their last applied record and the current time.

Monitoring `LastAppliedRecordLagMs` helps in detecting if the active controller is experiencing any delays in applying the latest metadata changes and ensures that the metadata state is consistent across all controllers.

A is incorrect because `ActiveControllerCount` represents the number of active controllers in the cluster, not the lag between the active controller and the metadata log.

B is incorrect because `EventQueueTimeMs` measures the time spent by requests in the controller event queue, not the lag between the active controller and the metadata log.

C is incorrect because `LastCommittedRecordOffset` represents the offset of the last committed record in the metadata log, but it does not provide information about the lag between the active controller and the metadata log.

## Question 10
What is the purpose of the `kafka.controller:type=KafkaController,name=OfflinePartitionCount` metric in KRaft mode?

- A. To track the number of partitions without an active leader
- B. To monitor the number of partitions that are under-replicated
- C. To measure the number of partitions that are not being consumed by any consumer
- D. To count the number of partitions that have exceeded their retention time

**Answer:** A

**Explanation:**
In KRaft mode, the metric `kafka.controller:type=KafkaController,name=OfflinePartitionCount` is used to track the number of partitions without an active leader.

When a partition loses its leader, either due to a broker failure or a network issue, it becomes offline and unavailable for read and write operations. The `OfflinePartitionCount` metric provides the count of such partitions that are currently offline and do not have an active leader.

Monitoring `OfflinePartitionCount` is important for detecting partition availability issues and ensuring that all partitions have active leaders. A non-zero value for this metric indicates that some partitions are offline and require attention.

B is incorrect because the number of under-replicated partitions is tracked by a separate metric, `kafka.server:type=ReplicaManager,name=UnderReplicatedPartitions`.

C is incorrect because the number of partitions not being consumed by any consumer is not directly related to the `OfflinePartitionCount` metric. Consumer lag and partition consumption are monitored using different metrics.

D is incorrect because the number of partitions that have exceeded their retention time is not related to the `OfflinePartitionCount` metric. Partition retention is managed based on the retention policy configuration and is not directly tied to partition leadership.