### Kafka Overview:
- **Kafka relies on ZooKeeper** for maintaining metadata, configuration data, and providing synchronization within distributed systems.
- **Auto topic creation** depends on the broker setting `auto.create.topics.enable`.
- **In Kafka, ordering is guaranteed** only within a partition, not across partitions.
- **A topic partition consists of segments** with two indexes: offset to position and timestamp to offset.
- **Topic, Partition and Offset** uniquely identifies a message in Kafka.
- **Replicas are passive**, they don't handle produce or consume request. Produce and consume requests get sent to the node hosting partition leader.
- **Point-to-point (request-response)** style will couple client to the server.

### Broker:

- **A Kafka Broker** is a JVM process that runs on a machine and hosts topics.
- Any Kafka broker can provide cluster **metadata to clients**.
- **In production, set the Java heap size to 6 GB** for optimal Kafka performance.
- **In a multi-broker Kafka cluster**, only the `broker.id` property needs to be unique for each node.
- **Garbage Collection** is used by Kafka brokers to handle the deletion of unused objects and tune performance.
- **Kafka brokers** handle all read and write requests for partitions and manage replication between partitions.
- **The Controller** is one of the brokers in a Kafka cluster responsible for managing the states of partitions and replicas.
- **Kafka brokers** communicate with each other using the Kafka protocol over TCP.
- **If a broker fails**, Kafka automatically fails over to one of the replicas, promoting it to be the new leader.
- **Broker configuration** is defined in the `server.properties` file, which includes settings like `broker.id`, `port`, `log.dirs`, `zookeeper.connect`, etc.
- **Kafka brokers** rely on ZooKeeper for maintaining their cluster state, like topics, partitions, replicas, leaders, ISRs, and more.
- **Each broker** has a unique integer identifier, the `broker.id`, which is used in various Kafka protocols and requests.
- `min.insync.replica`s does not impact producers when `acks=1` only when `acks=all`
- **Kafka brokers** handle authentication and authorization of clients based on the configured security protocols and ACLs.
- **Kafka brokers** expose metrics via JMX for monitoring their performance and health, like request rates, byte rates, CPU usage, etc.
- **Kafka brokers** can be configured to use rack awareness for improved fault tolerance, by spreading replicas across different racks.
- **Kafka brokers** can be horizontally scaled by adding more broker nodes to the cluster to increase storage and processing capacity.
- **OS page cache** is heavily used by Kafka brokers for caching log segments, which allows for efficient reads and writes.
- **Kafka brokers** persist all messages to disk and maintain an in-memory cache of messages to serve faster reads.
- **Quotas** can be configured on Kafka brokers to enforce limits on produce and fetch requests to control resource utilization.
- **Kafka brokers** support multiple listener configurations to allow different security protocols on different ports, like PLAINTEXT, SSL, SASL, etc.
- **Kafka brokers** can be configured with different log cleanup policies, like deletion or compaction, to manage disk space usage.
- **Log Segments:** Kafka brokers split log data into segments. Each segment is a large file, which is flushed to disk periodically.
- **Replica Fetchers:** Brokers have fetcher threads to pull data from leader replicas and replicate it to follower replicas.
- **Inter-Broker Communication:** Brokers use ZooKeeper to elect the controller and maintain metadata about the cluster.
- **Broker Metrics:** Commonly monitored metrics include `UnderReplicatedPartitions`, `OfflinePartitionsCount`, `RequestHandlerAvgIdlePercent`, and `NetworkProcessorAvgIdlePercent`.
- **Leader and Follower:** Each partition has a leader broker and follower brokers. The leader handles all reads and writes, while followers replicate the data.
- **Broker Startup:** During startup, brokers register themselves with ZooKeeper and load topic metadata to prepare for handling requests.
- **Kafka Protocol:** Brokers use a binary protocol for communication, which clients and other brokers utilize to interact with the cluster.
- **Broker Upgrades:** Brokers can be upgraded with zero downtime using a rolling upgrade process, ensuring continuous availability.
- In a 5 node ZooKeeper ensemble, **up to 2 servers can fail while still maintaining a quorum**. In a 9 broker, 4 can fail and still keep voting majority.
- ZooKeeper ensemble members communicate on **ports 2181, 2888, 3888** by default.
- The **Metadata request can be handled by any node**, enabling clients to discover the designated leader for topic partitions.
- With `replication.factor=3`, `min.insync.replicas=2`, `acks=all`, a `NOT_ENOUGH_REPLICAS` exception is thrown if **2 brokers are down**.
- Setting `unclean.leader.election.enable=true` allows **non-ISR replicas to become leader**, ensuring availability but risking data loss.
- ZooKeeper's behavior is governed by the ZooKeeper configuration file, which includes keywords like **clientPort, dataDir, and tickTime**.
- Kafka supports rack awareness for **fault tolerance** by configuring the `broker.rack` property to spread replicas across racks.
- Garbage collection in Kafka brokers is a **JVM process that automatically frees up memory and removes unused pointers to objects**, helping to improve overall performance.


### CLI:

- **Creating Topics:** Use `bin/kafka-topics.sh --create` to create topics.
- **Describing Topics:** Use `bin/kafka-topics.sh --describe` to get details about topics.
- **Listing Topics:** Use `bin/kafka-topics.sh --list` to list all topics in the cluster.
- **Deleting Topics:** Use `bin/kafka-topics.sh --delete` to delete a topic.
- **Consuming Messages:** Use `bin/kafka-console-consumer.sh` to consume messages from a topic.
- **Producing Messages:** Use `bin/kafka-console-producer.sh` to produce messages to a topic.
- **Managing Consumer Groups:** Use `bin/kafka-consumer-groups.sh` to manage consumer groups, including viewing offsets and lag.
- **Resetting Offsets:** Use `bin/kafka-consumer-groups.sh --reset-offsets` to reset the offsets for consumer groups.
- **Configuring Topics:** Use `bin/kafka-configs.sh` to alter topic configurations.
- **Viewing Configs:** Use `bin/kafka-configs.sh --describe` to view configurations of brokers, topics, or clients.
- **Running a Simple Producer Performance Test:** Use `bin/kafka-producer-perf-test.sh` to test producer performance.
- **Running a Simple Consumer Performance Test:** Use `bin/kafka-consumer-perf-test.sh` to test consumer performance.
- **Setting ACLs:** Use `bin/kafka-acls.sh --add` to set ACLs for users or hosts.
- **Listing ACLs:** Use `bin/kafka-acls.sh --list` to view the ACLs configured.
- **Removing ACLs:** Use `bin/kafka-acls.sh --remove` to remove ACLs.
- **Starting Kafka Connect:** Use `bin/connect-standalone.sh` for standalone mode or `bin/connect-distributed.sh` for distributed mode.
- **Monitoring Kafka Connect:** Use `bin/connect-distributed.sh --status` to check the status of Kafka Connect tasks and connectors.
- **Rebalancing Partitions:** Use `bin/kafka-reassign-partitions.sh` to perform partition reassignments.
- **Checking Reassignments:** Use `bin/kafka-reassign-partitions.sh --verify` to verify the status of partition reassignments.
- **Viewing Partition Reassignment Plans:** Use `bin/kafka-reassign-partitions.sh --generate` to generate a reassignment plan.
- **Running a Simple Consumer Group Test:** Use `bin/kafka-consumer-groups.sh --describe --group <group-id>` to check the status of consumer groups.
- **Listing Consumer Groups:** Use `bin/kafka-consumer-groups.sh --list` to list all consumer groups.
- **Decommissioning Brokers:** Use `bin/kafka-preferred-replica-election.sh` to initiate a preferred replica election.
- **Migrating ZooKeeper:** Use `bin/kafka-migration.sh` to help with migrating metadata from ZooKeeper to KRaft (Kafka's Raft-based metadata quorum).
- The Kafka CLI command to display the Kafka version is: `bin/kafka-console-producer.sh --version` or any `bin/kafka-*.sh` script with the `--version` parameter.
- To create a topic only if it does not already exist, use the `--if-not-exists` parameter with the `kafka-topics.sh` command.

### Administration:

- **For a safe data pipeline without message loss**, use `acks=all`, `replication.factor=3`, `min.insync.replicas=2`.
- With `replication.factor=3`, `min.insync.replicas=2`, `acks=all`, a `NOT_ENOUGH_REPLICAS` exception is thrown if 2 brokers are down.
- **Set `unclean.leader.election.enable=false`** to prevent data loss.  ("Consistency-Availability" trade-off)
- **Partitions with out-of-sync replicas** are considered under-replicated.
- **Use `min.insync.replicas`** to define the minimum number of replicas that must acknowledge a write for it to be considered successful.
- **Monitor under-replicated partitions** to ensure data durability and prevent data loss.
- **Use `auto.leader.rebalance.enable=true`** to automatically balance leadership across the cluster.
- **Configure `num.partitions` per topic** based on the desired parallelism and throughput.
- **Use `log.retention.hours` or `log.retention.bytes`** to control the data retention period.
- **Monitor disk usage** on brokers and add more disks or brokers when necessary.
- **Use `replica.lag.time.max.ms`** to control the maximum time a replica can lag behind the leader.
- **Upgrade Kafka brokers** one at a time, starting with the controller, to ensure a smooth rolling upgrade.
- **Use Kafka ACLs** to control access to topics and consumer groups.
- **Configure `max.message.bytes`** to control the maximum size of a message that can be produced.
- **Use `compression.type`** to enable compression for efficient storage and network usage.
- **Monitor the Kafka cluster** using tools like Kafka Manager, Prometheus, and Grafana.
- **Use Kafka Connect** for integrating Kafka with external systems like databases and filesystems.
- **Configure `zookeeper.connection.timeout.ms`** to control the maximum time to wait for a ZooKeeper connection.
- **Use Kafka Streams** for real-time data processing and aggregation.
- **Configure `producer.buffer.memory` and `consumer.fetch.max.bytes`** to tune producer and consumer performance.
- **Implement quotas** using `quota.producer.byte-rate` and `quota.consumer.byte-rate` to manage resource usage.
- **Set `log.cleanup.policy`** to either `delete` or `compact` to manage log retention policies. (`delete` is the default)
- **Use `controlled.shutdown.enable=true`** to ensure brokers shut down cleanly without data loss.
- **Monitor the `UnderReplicatedPartitions` metric** to track replication health.
- **Review and adjust JVM heap settings** to optimize broker performance (`-Xmx` and `-Xms` settings).
- **Implement rack awareness** by setting `broker.rack` to improve fault tolerance.
- **Regularly back up ZooKeeper data** to protect against data loss.
- **Enable TLS/SSL** for secure communication between brokers and clients.
- **Use `security.inter.broker.protocol`** to configure secure communication between brokers.
- **Configure `zookeeper.session.timeout.ms`** to manage ZooKeeper session timeouts.
- **Use `advertised.listeners`** to ensure brokers can be reached by clients.
- **Enable and monitor JMX** for broker metrics and performance data.

### Consumer:

- Consumers can **manually assign partitions using `assign()`** and **seek to specific offsets using `seek()`**.
- **The first consumer to join a group** becomes the group leader.
- **Consumer group rebalancing** reassigns partitions for a proportional share when members join/leave.
- **Kafka consumer partition assignment strategies** include `RangeAssignor`, `RoundRobinAssignor`, `StickyAssignor`, and `CooperativeStickyAssignor`.
- **Committed consumer group offsets** take precedence over `auto.offset.reset`.
- **For at-most-once**, commit offsets before processing messages.
- For at-least-once processing, **commit offsets after processing messages**.
- **Consumers can subscribe to multiple topics** via regex or a topic list.
- To achieve scaling out and scaling in for a ksqlDB cluster, you can start additional ksqlDB servers with the same configuration during live operations or stop the desired running servers, keeping at least one server running. The **maximum parallelism depends on the number of partitions**.
- In a multi-consumer setup, the **consumer group leader is responsible for assigning a subset of topic partitions to each consumer** in the group. The **heartbeat thread** is used to detect if a consumer has failed.
- If two consumers have the same group ID and subscribe to the same topic, **each consumer will be assigned a subset of the partitions** in the topic.
- To commit offsets, **consumers produce messages to a special `__consumer_offsets` topic** with the committed offset for each partition.
- For **at-most-once processing**, offsets should be committed **before** processing the data.
- **Adding more consumers to a consumer group** is the main way to scale data consumption from a Kafka topic.
- **Consumer offsets are stored** in the `consumer_offsets` topic, schemas are stored in the `_schemas` topic.
- **Use `max.poll.records`** to limit the number of records returned in a single call to `poll()`.
- **Use `enable.auto.commit=false`** for manual offset control.
- **Consumers with the same `group.id` form a consumer group** and share the partitions of subscribed topics.
- **Kafka consumers are not thread-safe** and should not be shared across threads.
- **Use `isolation.level=read_committed`** to only read transactional messages that have been committed.
- **Set `fetch.max.bytes` and `max.partition.fetch.bytes`** to control the amount of data fetched per partition.
- **Use `ConsumerRebalanceListener` to get notifications** when partitions are revoked or assigned during rebalancing.
- **Tune `fetch.min.bytes` and `fetch.max.wait.ms`** to balance between latency and throughput.
- **Set `max.poll.interval.ms`** to control the maximum time between poll invocations before the consumer is considered dead.
- **Use `seek()` to reset the consumer's position** to a specific offset.
- **Commit offsets asynchronously** using `commitAsync()` for better performance.
- **Handle `CommitFailedException`** when offset commit fails due to rebalancing or other errors.
- **Use `auto.offset.reset`** to control the behavior when there are no initial offsets or the current offset does not exist.
- **Configure `heartbeat.interval.ms`** to control the interval at which the consumer sends heartbeats to the group coordinator.
- **Use `session.timeout.ms`** to set the timeout for detecting consumer failures.
- **Monitor consumer lag** to ensure that the consumer is keeping up with the latest records.
- In a multi-consumer setup, the **consumer group leader is responsible for assigning a subset of topic partitions to each consumer** in the group.
- The **heartbeat thread** in a Kafka consumer is used to detect if a consumer has failed by sending periodic heartbeats to the broker.
- Consumer **fetch size** is the most important configuration for performance, controlled by `fetch.min.bytes` and `fetch.max.bytes`.
- The `wakeup()` method can be safely used from an external thread to **interrupt an active consumer operation**.
- Consumers **read data in order within each topic partition** and can **read from multiple partitions**.
- **Enable `client.id`** to provide a logical application name in logs and metrics for easier monitoring.
- **Set `interceptor.classes`** to add custom interceptors for additional logging, monitoring, or modifications to records.
- **Use `poll()` in a loop** to continuously consume messages from the Kafka broker.
- **Handle rebalances gracefully** by using `RebalanceListener` to perform necessary actions during partition assignment and revocation.
- **Calling `close()`** on consumer immediately triggers a partition rebalance as the consumer will not be available anymore.
- If there are no initial offsets or the current offset does not exist, setting `auto.offset.reset=none` will make the consumer **throw an exception**.
- When used with default options and no specified group ID, the `kafka-console-consumer` generates a **random consumer group**.

### Kafka Connect:

- Kafka Connect uses **workers** to execute connectors and tasks for reading data from or writing data to external systems.
- Kafka Connect provides a **REST API for managing connectors and tasks** in both standalone and distributed modes. The REST API server can be configured using the `listeners` configuration option.
- **Connectors** in Kafka Connect are responsible for breaking down the data copying job into **tasks** that can be distributed to workers. There are two types of connectors: **SourceConnectors for importing data and SinkConnectors for exporting data**.
- Confluent Cloud offers **fully managed connectors**, which can be leveraged by using the `ccloud-stack` utility to create a new Confluent Cloud instance.
- **Kafka Connect simplifies connector development**, deployment, and management, supporting distributed and standalone modes.
- **Distributed mode in Kafka Connect** handles automatic work balancing, scaling, and fault tolerance. Topics for offsets, configs, and statuses should be manually created.
- **Connector configurations** must include a unique name, max tasks, and the connector class.
- **Kafka Connect Source** is used to import data from external systems into Kafka.
- **The number of sink tasks** for a Kafka Connect connector should not exceed the number of partitions in the input topic(s).
- **max.tasks limits the number of tasks** per connector.
- **Kafka Connect Sink** is used to export data from Kafka to external systems.
- **The Kafka Connect REST API** allows managing and deploying connectors, and checking their status.
- **Use Single Message Transformations (SMTs)** to modify the data in-flight between the source and sink.
- **The Kafka Connect `key` and `value` converters** are used to serialize and deserialize data between Kafka and external systems.
- **Kafka Connect supports schema evolution** through the use of a schema registry like the Confluent Schema Registry.
- **Use the `tasks.max` parameter** to set the maximum number of tasks that should be created for the connector.
- **The Kafka Connect `offset` topic** stores the offsets for each connector, allowing for fault tolerance and recovery.
- **The Kafka Connect `status` topic** stores the current status of connectors and tasks.
- **Kafka Connect `dead letter queue`** can be used to handle connector errors and problematic records.
- **Kafka Connect supports custom connectors** through the implementation of the `SourceConnector` or `SinkConnector` interfaces.
- **Use the `connector.class` parameter** to specify the Java class for the connector.
- **Kafka Connect can be used for Change Data Capture (CDC)** to capture changes from databases and stream them into Kafka.
- **The Kafka Connect `config` topic** stores the configuration for each connector and task.
- **Kafka Connect can be scaled horizontally** by adding more worker nodes to the cluster.
- **Standalone mode in Kafka Connect** is simpler to set up but **lacks the fault tolerance and scalability of distributed mode**.
- Kafka Connect **supports exactly-once semantics** for **sink and source connectors** if they are designed to leverage the framework's capabilities.
- **Plugin discovery** in Kafka Connect is controlled by the `plugin.path` worker configuration, and the `service_load` strategy is fastest but requires verifying plugin compatibility.
- **Standalone mode in Kafka Connect** is simpler and requires less setup but lacks the fault tolerance and scalability of distributed mode.
- **Monitor Kafka Connect** using JMX metrics and integrate with monitoring tools like Prometheus and Grafana.
- **Restart connectors and tasks** through the REST API to recover from transient errors or apply configuration changes.
- **Ensure proper configuration** of `offset.storage.topic`, `config.storage.topic`, and `status.storage.topic` for reliable operation.
- **Use `value.converter.schemas.enable`** to control whether the schema is included with each message for Avro, JSON, and Protobuf.
- **Configure `key.converter` and `value.converter`** to specify the serialization format used by the connectors.
- **Leverage Kafka Connect transformations** to perform lightweight message modifications without needing custom code.
- Connector configurations are defined in the worker configuration file or REST requests as **key-value mappings**.
- Kafka Connect can provide **exactly-once semantics for sink and source connectors** if they are designed to take advantage of the framework's capabilities.

### Streams:

- **Tumbling time windows in Kafka Streams** are fixed-size, non-overlapping, and gap-less.
- **Kafka Streams achieves parallelism** by splitting the topology into tasks that handle a subset of partitions independently.
- The **maximum parallelism** of a Kafka Streams application is determined by the **number of partitions of the input topic(s)** being read.
- **In Kafka Streams, `map` creates an output stream**, `foreach` is a terminal operation, and `peek` is for side effects.
- **The Kafka Streams API** supports exactly-once delivery.
- **Stream-table duality:** A stream is a changelog of a table, and a table is a snapshot of a stream at a point in time.
- **When using a persistent state store** in Kafka Streams, close iterators to prevent memory issues.
- **The Kafka Streams API** supports KStream-to-KStream windowed joins and KTable-to-KTable non-windowed joins.
- **Stateless operations** don't require state, while stateful operations depend on state for processing: Branch, Filter, FlatMap, Foreach, GroupByKey, GroupBy, Map, Map (values only), Peek, Print, SelectKey, Table to Stream.
- **Stateful operations** : Aggregate, Count and Reduce. (Joins, windowing and custom pressesors)
- **Mounting a persistent volume for RocksDB** can dramatically speed up Kafka Streams recovery on restart.
- **reduce, join, count and aggregate** are stateful Kafka Streams operations.
- **KStream-KTable joins** output a KStream.
- Kafka Streams supports **at-least-once and exactly-once processing guarantees**.
- Kafka Streams applications run on **client machines at the periphery of a Kafka cluster**, not within the Kafka brokers or the cluster itself.
- To speed up recovery for a Kafka Streams application, the state can be stored on a **persistent volume**.
- A benefit of using `GlobalKTable` when joining input data is that the **input data does not need to be co-partitioned**.
- Kafka Streams **achieves parallelism by splitting the topology into tasks** that are executed by threads across application instances. The **maximum parallelism is determined by the number of partitions** of the input topic(s).
- When repartitioning in Kafka Streams, the framework **writes the repartitioned data to a new topic with new keys and partitions**, creating two independent subtopologies.
- **Kafka Streams exactly-once semantics** apply to Kafka-to-Kafka flows only.
- **KStream-GlobalKTable join** does not require input topics to have the same number of partitions, unlike other join types.
- **Kafka Streams uses the `application.id` config** as a prefix for internal topics like repartition and state topics.
- **Kafka Streams supports Interactive Queries** to query the state of a running application.
- **Hopping time windows in Kafka Streams** have a fixed size but can overlap, emitting results at a specified interval.
- **Sliding time windows in Kafka Streams** are similar to hopping windows but emit results every time a new event enters or exits the window.
- **Session windows in Kafka Streams** group events by key and session, with sessions having a defined inactivity gap.
- Kafka Streams should be used for **transforming data from one Kafka topic to another**, as it is simpler and more powerful than using a consumer and producer directly.
- To effectively **manage the size of state stores in Kafka Streams**, consider using **windowed stores, producing tombstones, or implementing a custom TTL-based cleanup mechanism**.
- **Kafka Streams supports custom SerDes** for serializing and deserializing data.
- **Kafka Streams requires at least one input topic and one output topic** for processing data.
- **The Kafka Streams DSL** provides a high-level API for common stream processing operations.
- **The Kafka Streams Processor API** provides a low-level API for custom stream processing logic.
- **Kafka Streams supports fault-tolerant state** through the use of changelog topics and state stores.
- **The Kafka Streams `KStream` abstraction** represents an unbounded stream of records.
- **The Kafka Streams `KTable` abstraction** represents a changelog stream, where each record represents an update.
- **The Kafka Streams `GlobalKTable` abstraction** represents a fully replicated, non-partitioned changelog stream.
- Use KStream when you need to process each record independently, perform stateless transformations, or handle unbounded data.
- Use KTable when you need to perform aggregations, joins, or maintain a materialized view of the latest values for each key.
- When joining streams/tables, **input data must be co-partitioned with the same number of partitions**. (Except for GlobalKTable)
- **Branch, FlatMapValues, GroupBy** are examples of stateless operations in Kafka Streams.
- To convert a KStream to a KTable, options include `KStream.toTable()`, **writing to Kafka and reading as KTable**, or performing a dummy aggregation.
- For Kafka Streams output topics, it's recommended to set `cleanup.policy=compact` to align with **KTable semantics**.

### KSQL:

- ksqlDB is a dialect inspired by ANSI SQL but introduces differences like **"windowing" for streaming data processing**.
- ksqlDB **doesn't support structured keys**, making it impossible to create a stream from a windowed aggregate.
- **ksqlDB simplifies stream processing applications** on Kafka by allowing developers to write SQL queries. Key use cases include materialized caches, streaming ETL, and event-driven microservices.
- **ksqlDB supports windowing operations** like `TUMBLING`, `HOPPING`, and `SESSION` windows for aggregations over time.
- **SHOW STREAMS and EXPLAIN statements** in ksqlDB communicate directly with the ksqlDB server, not Kafka.
- **Idle ksqlDB servers** consume few resources. Only servers corresponding to the number of partitions actively process a query.
- **ksqlDB supports exactly-once processing**, configurable with the `processing.guarantee` setting.
- **Adding a ksqlDB server to an existing cluster** requires matching `bootstrap.servers` and `ksql.service.id` settings.
- **The DESCRIBE EXTENDED statement in ksqlDB** helps check compatibility between the `VALUE_FORMAT` of a stream and the format of topic records.
- **The default port for the KSQL server is 8088**.
- **KSQL CREATE STREAM/TABLE AS SELECT queries** write to Kafka topics.
- The `bootstrap.servers` configuration in ksqlDB specifies the **initial connection point to the Kafka cluster** as a list of host and port pairs.
- ksqlDB allows you to **query, read, write, and process data in Kafka** in real-time and at scale using **intuitive SQL-like syntax** without requiring proficiency in Java or Scala or installing a separate processing cluster.
- ksqlDB can be deployed in **headless mode** (recommended for production) or **interactive mode**.
- ksqlDB stores **metadata in the config topic** in headless mode, containing the ksqlDB properties provided during the initial startup of the application.
- In interactive mode, securing the **ksqlDB command topic is crucial for protecting sensitive metadata** related to DDL statements and TERMINATE queries.
- ksqlDB **supports Avro, Protobuf, JSON, and delimited formats** by specifying the desired format and configuring `ksql.schema.registry.url` to point to Schema Registry.
- **Use SET `auto.offset.reset`='earliest'** for KSQL to read a topic from the start.
- **KSQL-related data and metadata are stored** in Kafka topics, not in Zookeeper, databases, or Schema Registry.
- In interactive mode, **securing the ksqlDB command topic is crucial** for protecting sensitive metadata related to **DDL statements and TERMINATE queries**.
- ksqlDB **supports Avro, Protobuf, and JSON formats** by specifying the desired format (e.g., `VALUE_FORMAT='AVRO'`) and configuring `ksql.schema.registry.url` to point to Schema Registry.
- **ksqlDB supports Pull and Push queries**. Pull queries are one-time queries that return a result immediately, while Push queries are continuous queries that output results to a new stream or table.
- **The ksqlDB REST API** allows interacting with ksqlDB servers programmatically.
- **ksqlDB supports User Defined Functions (UDFs)** for custom processing logic.
- **ksqlDB queries are scalable and fault-tolerant** since they leverage the underlying Kafka infrastructure.
- **The `CREATE STREAM` statement** in ksqlDB creates a new stream from a Kafka topic or another stream.
- **The `CREATE TABLE` statement** in ksqlDB creates a new table from a Kafka topic or another table.
- **The `INSERT INTO` statement** in ksqlDB writes records into an existing stream or table.
- **The `CREATE CONNECTOR` statement** in ksqlDB creates a new Kafka Connect connector.
- **ksqlDB supports JOIN operations** between streams and tables, including `INNER JOIN`, `LEFT JOIN`, and `OUTER JOIN`.
- To use Avro data with ksqlDB, **Schema Registry must be installed** and `ksql.schema.registry.url` configured to point to it.
- The ksqlDB command topic stores metadata for **persistent queries based on `CREATE STREAM AS SELECT` and `CREATE TABLE AS SELECT`**.
- Maximum parallelism in ksqlDB depends on the **number of topic partitions**.

### Metrics:
- **Confluent Control Center** enables centralized management and monitoring of Kafka components.
- **Important metrics** to monitor include `UnderReplicatedPartitions`, `ActiveControllerCount`, `OfflinePartitionsCount`, `RequestHandlerAvgIdlePercent`, `NetworkProcessorAvgIdlePercent`, and `IsrExpandsPerSec`.
- **Monitor consumer lag** using `records-lag-max` to track how many messages the consumer is behind.
- **Kafka exposes metrics via JMX** (Java Management Extensions) for monitoring.
- **Use Kafka's built-in JmxTool** to dump JMX metrics periodically for analysis.
- **Monitor the `BytesInPerSec` and `BytesOutPerSec` metrics** to track the inbound and outbound byte rate of brokers.
- **The `MessagesInPerSec` metric** indicates the rate at which messages are being produced to brokers.
- **Monitor the `UnderMinIsrPartitionCount` metric** to track partitions with insufficient in-sync replicas.
- **The `PartitionCount` metric** shows the total number of partitions across all topics in the cluster.
- **The `LeaderCount` metric** indicates the number of partitions for which a broker is the leader.
- **Monitor the `RequestQueueSize` and `ResponseQueueSize` metrics** to ensure brokers can keep up with client requests.
- **The `ProduceTotalTimeMs` and `FetchTotalTimeMs` metrics** track the total time taken for produce and fetch requests, respectively.
- **Use the `FetchMessageConversionsPerSec` metric** to monitor the rate of message format conversions during fetch requests.
- **The `ZooKeeperRequestLatencyMs` metric** tracks the latency of ZooKeeper requests made by Kafka.
- **Monitor the `NetworkProcessorAvgIdlePercent` metric** to ensure the network thread is not a bottleneck.
- **The `ControllerState` metric** indicates whether a broker is currently the active controller.
- **Use Prometheus and Grafana** for collecting, storing, and visualizing Kafka metrics.
- **The Confluent Platform** includes built-in dashboards for monitoring Kafka clusters, Connect, ksqlDB, and Schema Registry.
- **Datadogs Kafka integration** provides pre-built dashboards and alerts for monitoring Kafka performance and health.
- **Cloudwatch and CloudTrail** are used for monitoring Kafka clusters running on AWS.
- **Azure Monitor** is used for monitoring Kafka clusters running on Azure.

### Producer:
- **The most important producer configurations** are `acks`, `compression`, and `batch.size`.
- **Producers send messages to the leader** of a topic partition. Same keys go to the same partition.
- **The Kafka Producer workflow** includes `ProducerRecord`, `Serializer`, and `Partitioner`, but not `Deserializer`.
- **To manually commit offsets**, use the `commitSync()` API.
- **Set `bootstrap.servers` to multiple host:port pairs** for Kafka broker fault tolerance.
- **bootstrap.servers, key.serializer and value.serializer** are mandatory for producers.
- **Setting `max.in.flight.requests.per.connection > 1`** with retries enabled can lead to out-of-order messages.
- **linger.ms increases the chance of batching** in producers.
- The `send()` method returns a **Future object with RecordMetadata**. Using `Future.get()` **waits for a reply from Kafka** before continuing and **throws an exception if the record is not sent successfully**.
- Setting `enable.idempotence=true` is a producer configuration that can be used to **guarantee a stable and safe pipeline without processing duplicate messages**.
- To send binary data through the REST Proxy, the **producer** needs to encode the data into base64.
- When creating a `ProducerRecord`, the **topic and value are mandatory**, while the key and partition are optional.
- **Enabling compression and increasing batch.size** can improve producer throughput.
- **`Message_TOO_LARGE`, `INVALID_REQUIRED_ACKS`, and `TOPIC_AUTHORIZATION_FAILED`** are examples of non-retriable errors for Kafka producers, while `NOT_ENOUGH_REPLICAS` and `NOT_LEADER_FOR_PARTITION` are retriable.
- **Using producer.send(record).get()** will decrease throughput by waiting for a reply from Kafka.
- **Setting compression.type=snappy** for a producer means consumers have to decompress the data. Kafka brokers transfer compressed data as-is.
- **KafkaProducer may throw `SerializationException` or `BufferExhaustedException`** before sending the record. It handles large messages by throwing `MessageSizeTooLargeException` without retries if exceeding max size.
- **Setting `enable.idempotence=true`** helps prevent duplicate messages caused by network issues between the producer and broker.
- **The `acks` configuration** determines the level of acknowledgement required from the broker before considering a write successful.
- **The `buffer.memory` configuration** sets the total amount of memory the producer can use to buffer records waiting to be sent to the server.
- **The `retries` configuration** sets the number of times the producer will retry a failed request before giving up.
- **The `max.block.ms` configuration** sets the maximum amount of time the producer will block when calling `send()` or `partitionsFor()`.
- **The `request.timeout.ms` configuration** sets the maximum amount of time the producer will wait for a response from the server when sending a request.
- **The `max.request.size` configuration** sets the maximum size of a request in bytes.
- **The `transactional.id` configuration** enables idempotent and transactional delivery.
- **Setting `compression.type` to `lz4`** provides a good balance of compression ratio and CPU usage.
- **The `partitioner.class` configuration** allows specifying a custom partitioner for determining which partition a record should be sent to.
- **Producers can be created using the `KafkaProducer` class** in the Kafka Java API.
- `RecordTooLargeException` is a **non-retriable exception**, meaning the message will not be sent again.
- The `send()` method returns a **Future object with RecordMetadata**.
- To receive leader acknowledgment of writes, set `acks=1`.
- **Increasing batch size (`batch.size`) and enabling compression (`compression.type`)** can improve producer throughput when the batches are completely full.
- Setting **`linger.ms`** to a higher value (e.g., 10 ms) **increases the chances of batching messages** by allowing the producer to wait before sending a batch.
- **Custom partitioning** can be implemented to optimize message distribution across partitions based on specific requirements, such as dedicating a partition to a high-volume customer.


### Schema Registry:
- **Supported schema formats in Confluent Platform** include Avro, JSON, and Protobuf.
- **For Avro BACKWARD compatibility**, allowed changes are deleting fields and adding optional fields.
- **For Avro FORWARD compatibility**, allowed changes are adding fields with defaults and deleting optional fields.
- **For Avro FULL compatibility**, only adding optional fields is allowed.
- **Avro primitive types** include null, boolean, int, long, float, double, bytes, and string.
- **Avro logical types** include date, time, timestamp, and decimal.
- **Schema Registry supports HTTP and HTTPS client protocols**.
- **The Schema Registry stores schemas in the internal `_schemas` Kafka topic**, not in an embedded database or Zookeeper.
- **Avro deserializer fetches missing schemas from the registry**.
- **Adding a field without a default is a forward compatible Avro schema change**.
- **Avro SpecificRecord classes** are generated from an Avro schema plus a Maven/Gradle plugin, not just the schema alone.
- The **Confluent Schema Registry default compatibility type** is `BACKWARD`, and `FULL` compatibility allows adding or removing **optional fields only**.
- **Queries** in Schema Registry can be tested using **cURL**, such as `curl -X GET http://localhost:8081/schemas/ids/1` to fetch a schema by its global ID.
- **The Schema Registry provides a REST API** for registering, retrieving, and managing schemas.
- **The Schema Registry uses a unique subject name per topic** to identify the schema associated with the data.
- **The Schema Registry supports schema evolution** by allowing compatible schema changes.
- **The Schema Registry checks compatibility** based on the compatibility type set for the subject (e.g., BACKWARD, FORWARD, FULL).
- **The Schema Registry can be deployed in a multi-node setup** for high availability and fault tolerance.
- **The Schema Registry integrates with Kafka Connect** for automatic schema management in source and sink connectors.
- **The Schema Registry integrates with ksqlDB** for schema inference and schema evolution in ksqlDB applications.
- **The Schema Registry UI** provides a web-based interface for viewing, searching, and managing schemas.
- **The Schema Registry Client** is a Java library for interacting with the Schema Registry from Kafka clients.
- **The Schema Registry Maven Plugin** generates Java classes from Avro schemas for use in Kafka producers and consumers.
- **The Schema Registry supports multiple subjects per topic**, allowing different schemas for keys and values.
- **Confluent Replicator** can be used to replicate schemas between Schema Registry clusters.
- **Avro deserializer fetches missing schemas from the registry**.
- Adding a field without a default is a **forward compatible Avro schema change**.

### Security:
- **Encryption is configured** using listener configuration and managing truststores/keystores to protect data in transit.
- **Kafka security features are optional**, supporting a mix of authenticated, unauthenticated, encrypted, and non-encrypted clients.
- **To allow a user to read/write a topic**, add an ACL with `--allow-principal` and `--allow-host` for read/write operations.
- **SASL can be used with PLAINTEXT or SSL**. If `SASL_SSL`, SSL must also be configured.
- **ACLs are stored in the Zookeeper node /kafka-acls/ by default**.
- **SAML is not a valid authentication mechanism in Kafka**. Valid options are SASL/GSSAPI, SASL/SCRAM, and SSL.
- **Kafka clients use HTTPS (SSL/TLS) to securely connect to the Confluent REST Proxy**.
- **Enabling SSL encryption in Kafka disables the zero-copy optimization** since data must be loaded into the JVM to encrypt/decrypt.
- Kafka supports authentication using **SSL certificates, SASL/PLAIN, SASL/SCRAM, SASL/GSSAPI (Kerberos)**.
- **SASL/PLAIN and SASL/SCRAM** provide username/password-based authentication, while **SASL/GSSAPI** integrates with Kerberos.
- The security protocol of each listener is defined in the `listener.security.protocol.map` configuration, with options like **PLAINTEXT, SSL, SASL_PLAINTEXT, SASL_SSL**.
- **Kafka supports authorization using ACLs (Access Control Lists)** to control access to resources like topics and consumer groups.
- **Encryption of data at rest can be achieved using disk encryption** or by configuring Kafka to encrypt its data files.
- **Kafka supports SSL/TLS encryption for client-broker and broker-broker communication**.
- **ACLs in Kafka are defined using a combination of principal, host, operation, and permission**.
- **Kafka supports HTTPS for securing communication with the Kafka Connect REST API and Schema Registry API**.
- **Confluent Control Center supports HTTPS** for securing its web interface and API endpoints.
- **Kafka supports delegating authentication and authorization to external systems** like LDAP or Active Directory using plugins.
- **Quotas can be used to limit the resource consumption** of clients, such as the amount of data they can produce or consume.
- **Kafka supports encrypting sensitive configuration values** using the `kafka-configs` command-line tool.
- **The Confluent Platform Security Plugin** provides additional security features like Audit Logs, Quotas, and LDAP integration.
- When securing a running Kafka cluster, the **recommended approach** is to enable security in phases: **client-broker connection first, then broker-broker, and finally closing the PLAINTEXT port**.
- **Host name verification** in Kafka is the process of checking the certificate presented by the server against the actual hostname or IP address to prevent man-in-the-middle attacks. It can be disabled by setting `ssl.endpoint.identification.algorithm` to an empty string.
- Kafka supports authenticating to ZooKeeper with **SASL and mTLS individually or both together**. When using mTLS alone, every broker and CLI tool should identify itself with the **same Distinguished Name (DN)** for proper ACL'ing.
- The Confluent Server **Authorizer** can be used to capture, protect, and preserve authorization activity into topics in a Kafka cluster, helping to track user and application access across the platform through **audit logs**.

### ZooKeeper:  (Being replaced by KRaft, less important now)
- **ZooKeeper keeps track of znodes**, which have a path, can store data, and be persistent or ephemeral. Renaming znodes is not supported.
- **In a 5 node ZooKeeper ensemble, up to 2 servers can fail while still maintaining a quorum**.
- **ZooKeeper ensemble members communicate on ports 2181, 2888, 3888 by default**.
- **For ZooKeeper config tickTime=2000, initLimit=20, syncLimit=5**, follower connection timeout is 40 sec (2000ms * 20 ticks).
- **ZooKeeper uses a leader-follower architecture**, where one node is elected as the leader and the others are followers.
- **ZooKeeper uses a consensus algorithm called Zab (ZooKeeper Atomic Broadcast)** to ensure consistency across the ensemble.
- **ZooKeeper stores its data in memory** and writes transaction logs and snapshots to disk for persistence.
- **ZooKeeper clients connect to a single ZooKeeper server** and can failover to another server if the connection is lost.
- **ZooKeeper is used by Kafka for storing metadata** like topics, partitions, replicas, and controller information.
- **The `zookeeper.connect` parameter in Kafka** specifies the connection string for the ZooKeeper ensemble.
- **The `zookeeper.session.timeout.ms` parameter in Kafka** sets the timeout for ZooKeeper sessions.
- **The `zookeeper.connection.timeout.ms` parameter in Kafka** sets the maximum time to wait for a ZooKeeper connection.
- **ZooKeeper uses a hierarchical namespace** similar to a file system, with paths separated by slashes (/).
- **ZooKeeper supports watches** that allow clients to receive notifications when a znode changes.
- **ZooKeeper has a command-line interface (CLI)** for interacting with the ZooKeeper ensemble.
- **The `zkCli.sh` script** is used to start the ZooKeeper CLI and execute commands.
- **The `zoo.cfg` file** is used to configure ZooKeeper, including settings like `tickTime`, `dataDir`, and `clientPort`.
- **ZooKeeper can be used for distributed locking, leader election, and configuration management** in distributed systems.
- **Confluent Kafka distributions include a bundled ZooKeeper** for convenience, but a separate ZooKeeper ensemble is recommended for production.
- **ZooKeeper requires a majority (quorum) of servers to be available** for the ensemble to be operational.
- Running Kafka in production, a machine should have at least **32 GB of RAM** as a decent choice for storing and caching messages.
- In a ZooKeeper ensemble with 9 servers, **up to 4 servers can fail** while still maintaining a quorum.
- The command to start the ZooKeeper service is: `bin/zookeeper-server-start.sh config/zookeeper.properties`.

### KRaft Configuration in Confluent Platform

#### Hardware and JVM Requirements
- **Minimum of 4 GB of RAM**
- **Dedicated CPU core** should be considered when the server is shared
- An **SSD disk at least 64 GB** in size is highly recommended
- **JVM heap size of at least 1 GB** is recommended
### Configuration Files
- Example KRaft configuration files are located in `/etc/kafka/kraft/` after installing Confluent Platform
 - `broker.properties`: Settings for broker-only servers
 - `controller.properties`: Settings for controller-only servers
 - `server.properties`: Settings for combined broker and controller servers (not supported for production)
#### Metrics Reporter
- The metrics reporter must be **enabled on each broker and controller** in KRaft mode to see broker metrics in Confluent Control Center
- Uncomment the relevant lines in the properties file
#### Socket Server Settings
- `listeners`: Must be configured for controllers, consistent with `controller.quorum.voters` value
- `controller.listener.names`: Required for KRaft mode, specifying listeners used by the controller
#### Log Settings
- `log.dirs`: Should list only one log directory for KRaft mode, as JBOD is not currently supported
- `num.partitions`: Sets the default number of log partitions per topic for brokers (ignored by controllers)
#### Metadata Retention Settings
- `metadata.log.dir`: Specifies the location of the metadata log for KRaft clusters (defaults to the first `log.dirs` directory if not set)
- `metadata.max.idle.interval.ms`: Sets how often the active controller writes no-op records to the metadata partition (default 500 ms)
#### Settings for Other Components
- When using KRaft instead of ZooKeeper, current, non-deprecated configuration settings must be used for:
 - Clients and services
 - Schema Registry
 - Administrative tools
 - Retrieving the Kafka cluster ID
#### Generating and Formatting IDs
- Before starting Kafka, use the `kafka-storage` tool to:
 - Generate a cluster ID with `random-uuid`
 - Format each node with the `format` command
#### Debugging Tools
- Kafka provides tools for debugging KRaft mode clusters:
 - `kafka-metadata-quorum`: Describe runtime status
 - `kafka-dump-log`: Debug log segments
 - `kafka-metadata-shell`: Inspect the metadata partition
#### Monitoring Metrics
- Key metrics to monitor for KRaft mode:
 - KRaft quorum metrics (e.g., `append-records-rate`, `commit-latency-avg`)
 - Controller metrics (e.g., `ActiveBrokerCount`, `LastAppliedRecordOffset`)
 - Broker metadata metrics (e.g., `last-applied-record-offset`, `metadata-load-error-count`)

### Topic:
- **Auto topic creation** uses broker config for `num.partitions` and `default.replication.factor`.
- **Dynamic topic configs are stored in Zookeeper**.
- **Kafka Connect supports up to 1 task per input topic partition**.
- **Producing with keys** allows influencing the partitioning of messages to ensure ordering within each partition.
- **To produce data to a topic**, a producer must provide any broker from the cluster and the topic name. The partitions list is not required.
- **Create a topic with** `bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 3 --partitions 3 --topic test`.
- **The number of partitions in a Kafka topic can only be increased**, not decreased, using the kafka-topics.sh command.
- **Configure topic-level retention.ms**, not just broker-level log.retention.ms, to set retention for a specific topic to 1 hour (3600000 ms).
- **Only the leader replica handles produce and consume requests** for a partition. Follower replicas do not actively serve clients.
- **To find partitions without a leader**, use: `kafka-topics.sh --bootstrap-server localhost:9092 --describe --unavailable-partitions`.
- **Partition rebalance for a consumer group** is triggered by increasing partitions, adding/removing consumers, or consumer shutting down.
- **If multiple log retention configs are set**, the smaller unit size takes precedence.
- **Adding partitions causes new records with the same key to potentially go to different partitions**. Existing records stay put.
- **Tombstone records with null value will remove all messages with that key from the log on compaction**.
- **Log compaction is triggered when a segment closes if the dirty ratio threshold is met**. Not on every message or flush.
- **Kafka messages are immutable and cannot be modified after they are written to the log**.
- **The "same key to same partition" rule breaks down if the number of partitions for the topic changes**, as this changes the key hashing.
- **For Kafka Streams output topics, set `cleanup.policy=compact`** to align with KTable semantics that require log compaction.
- **With fewer topic partitions than consumer group members**, some consumers will remain idle, limited by the number of partitions.
- **Active-passive mirroring** is when a topic is replicated from an active region to a passive region used only for read-only purposes like analytics.
- **With 5 brokers, 10 partitions, 3 replicas, a client is allowed 5 MB/s maximum throughput** if each broker has a 1 MB/s quota.
- **Any broker can handle metadata requests**.
- **Consumers commit offsets by interacting with the Group Coordinator broker**, not by writing directly to the `consumer_offsets` topic.
- **The Controller is a broker elected by ZooKeeper** to be responsible for partition leadership assignment (in addition to normal tasks).
- **Producers automatically handle broker leader changes** by requesting new leaders from any broker and resuming production.
- `unclean.leader.election.enable=true` allows non ISR replicas to become leader, ensuring availability but losing consistency as data loss might occur
- A Kafka broker automatically creates a topic if a producer sends a message, a consumer reads a message, or a client requests metadata for the topic, when `auto.create.topics.enable` is set to **true**.
- Adding partitions causes **new records with the same key to potentially go to different partitions**, while existing records stay put.
- Kafka topics are always **multi-producer and multi-subscriber**, and you can have **as many topics as you want**.
- To configure a **topic-level retention period**, set the `retention.ms` property for the specific topic (**10 days = 864000000 ms**).
- Log compaction in Kafka ensures that at least the **last known value for each message key** is retained within a topic partition, which is useful for restoring state after crashes or rebuilding caches.
- With log compaction, **tombstone records with null values** will remove all messages with that key from the log.


### Other:
- **Serialization converts objects to byte streams** for transmission, while deserialization does the opposite.
- **Unit tests verify** isolated code blocks work as expected.
- **After adding partitions**, there's no guarantee that old and new messages with the same key will be on the same partition.
- **Kafka uses TCP** for high-performance communication between servers and clients.
- **In point-to-point messaging**, messages are exchanged between senders and receivers using a queue.
- **Active-passive mirroring** is when a topic is replicated from an active region to a passive region used only for read-only purposes like analytics.
- **To reduce consumer rebalance time from 10s to 3s with 12 consumers**, decrease `session.timeout.ms` to 3s and decrease `heartbeat.interval.ms`.
- **To guarantee at-least-once processing**, do not commit offsets until successfully processing the failed record, rather than committing earlier offsets.
- **Consumers in the same group read mutually exclusive partitions**, not mutually exclusive offsets or all data from all partitions.
- **The Kafka Producer is thread-safe and can be shared across threads**, but the Consumer is not thread-safe and should be used in one thread.
- Kafka uses **Java and Scala** as its primary programming languages.
- When evaluating a stream processing system, global considerations include the **availability of clean APIs and abstractions** and the system's **community support**, in addition to use-case-specific considerations.
