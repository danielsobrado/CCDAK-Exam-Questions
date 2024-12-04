## Question 1: Viewing Kafka Topic Configuration

**Question:**  
How can you view the current configuration of a Kafka topic?

A. Use the kafka-topics.sh --describe command
B. Use the kafka-configs.sh --describe command
C. Use the zookeeper-shell.sh command to navigate to the topic's configuration znode
D. Look in the Kafka broker's log files for the topic configuration

**Answer: B**

**Explanation:**

To view the current configuration of a Kafka topic, you should use the kafka-configs.sh --describe command. This tool is designed to manage and display configurations for Kafka entities, including topics, brokers, and clients.

When you run:
```bash
kafka-configs.sh --bootstrap-server <broker> --entity-type topics --entity-name <topic-name> --describe
```

It retrieves and displays the configuration properties for the specified topic, such as:
- Retention policy (retention.ms)
- Cleanup policy (cleanup.policy)
- Compression type

**Option A**: The kafka-topics.sh --describe command provides metadata about the topic, such as partition count, replication factor, and leadership information, but does not show configuration properties like retention settings.

**Option C**: Using zookeeper-shell.sh to inspect topic configurations directly in Zookeeper is not recommended. This approach is outdated, especially as newer Kafka versions no longer rely on Zookeeper.

**Option D**: Topic configurations are not stored in the Kafka broker's log files. These files contain runtime logs, not configuration data.

## Question 2

What is the default behavior of the `kafka-console-consumer` when no consumer group is specified?

- A. It joins a random consumer group
- B. It creates a new consumer group with a generated name
- C. It fails with an error indicating that a consumer group must be specified
- D. It consumes messages without joining any consumer group

**Answer:** B

**Explanation:**
When using the `kafka-console-consumer` CLI tool to consume messages from a Kafka topic, if you don't explicitly specify a consumer group using the `--group` option, the tool's default behavior is to create a new consumer group with a generated name.

The `kafka-console-consumer` automatically generates a unique consumer group name for each instance of the tool that is run without a specified group. The generated group name typically follows a pattern like `console-consumer-<random-string>`, where `<random-string>` is a randomly generated string to ensure uniqueness.

- B. creating a new consumer group for each instance, the `kafka-console-consumer` ensures that multiple instances of the tool can consume messages independently from the same topic without interfering with each other's offsets or causing rebalances.

Statement A is incorrect because the tool does not join a random existing consumer group. It creates a new group with a generated name.

Statement C is incorrect because the tool does not fail with an error when no consumer group is specified. It handles this scenario by creating a new group.

Statement D is incorrect because the `kafka-console-consumer` always joins a consumer group, even if it's a newly created one with a generated name. It does not consume messages without being part of a group.

## Question 3

How does the `kafka-console-consumer` behave when you specify the `--from-beginning` option?

- A. It starts consuming messages from the earliest available offset in the assigned partitions
- B. It starts consuming messages from the latest available offset in the assigned partitions
- C. It starts consuming messages from a specific offset that you provide
- D. It starts consuming messages from a random offset in the assigned partitions

**Answer:** A

**Explanation:**
When you run the `kafka-console-consumer` CLI tool with the `--from-beginning` option, it starts consuming messages from the earliest available offset in the assigned partitions.

- B. default, when a consumer starts consuming from a topic, it begins from the latest offset, which means it will only receive new messages that are produced after the consumer started. However, when you specify the `--from-beginning` option, the consumer will seek to the earliest available offset in each assigned partition and start consuming messages from there.

This option is useful when you want to consume all the messages in a topic, including the older messages that were produced before the consumer started. It allows you to process the entire history of messages in the topic.

Keep in mind that consuming from the beginning can result in a large number of messages being processed, especially if the topic has a long retention period or has been receiving messages for a significant time.

Statement B is incorrect because the `--from-beginning` option does not start consuming from the latest offset. It starts from the earliest offset.

Statement C is incorrect because the `--from-beginning` option does not allow you to specify a specific offset to start consuming from. It always starts from the earliest available offset.

Statement D is incorrect because the `--from-beginning` option does not start consuming from a random offset. It deterministically starts from the earliest offset in each assigned partition.

## Question 4

What happens when you run multiple instances of the `kafka-console-consumer` with the same consumer group?

- A. The instances will consume messages independently, each receiving a copy of every message
- B. The instances will collaborate and distribute the partitions among themselves for parallel consumption
- C. The instances will compete for messages, and each message will be consumed by only one instance
- D. The instances will consume messages in a round-robin fashion, with each instance receiving a subset of messages

**Answer:** B

**Explanation:**
When you run multiple instances of the `kafka-console-consumer` CLI tool with the same consumer group, the instances will collaborate and distribute the partitions among themselves for parallel consumption.

In Kafka, consumers within the same consumer group coordinate with each other to share the work of consuming messages from the topic partitions. When multiple consumers belong to the same group, Kafka assigns each partition to one consumer in the group. This assignment is dynamic and can change over time as consumers join or leave the group.

Here's how it works:

1. When the first consumer instance starts, it becomes the group leader and triggers a rebalance. It is assigned a subset of the topic partitions.
2. When subsequent consumer instances start with the same group, they join the group and trigger a rebalance. The partitions are redistributed among all the consumers in the group.
3. Each consumer instance will consume messages from its assigned partitions independently. Messages from a single partition are processed by only one consumer instance.
4. If a consumer instance fails or is terminated, the partitions it was consuming are redistributed among the remaining consumers in the group during a rebalance.

This collaborative consumption model allows for parallel processing of messages, improved throughput, and fault tolerance. The workload is distributed among the consumer instances, and if one instance fails, the others can take over its partitions.

Statement A is incorrect because the instances do not consume messages independently or receive a copy of every message. They collaborate and divide the partitions among themselves.

Statement C is incorrect because the instances do not compete for messages. Each message is consumed by only one instance, but the instances work together to distribute the partitions.

Statement D is incorrect because the instances do not consume messages in a round-robin fashion. Each instance is assigned specific partitions and consumes messages only from those partitions.

## Question 5

How can you create a topic named "test" with 3 partitions and a replication factor of 2 using the Kafka CLI?

- A. kafka-topics.sh --create --zookeeper localhost:2181 --topic test --partitions 3 --replication-factor 2
- B. kafka-topics.sh --create --bootstrap-server localhost:9092 --topic test --partitions 3 --replication-factor 2
- C. kafka-console-producer.sh --broker-list localhost:9092 --topic test --partitions 3 --replication-factor 2
- D. kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --partitions 3 --replication-factor 2

**Answer:** B

**Explanation:**
To create a topic using the Kafka CLI, you should use the `kafka-topics.sh` command with the `--create` option. The `--bootstrap-server` option is used to specify the Kafka broker(s) to connect to, while `--zookeeper` is deprecated. The `--partitions` and `--replication-factor` options are used to set the desired number of partitions and replication factor for the topic.

## Question 6

Which command can you use to list all the topics in a Kafka cluster?

- A. kafka-topics.sh --list --zookeeper localhost:2181
- B. kafka-topics.sh --list --bootstrap-server localhost:9092
- C. kafka-console-producer.sh --list --broker-list localhost:9092
- D. kafka-console-consumer.sh --list --bootstrap-server localhost:9092

**Answer:** B

**Explanation:**
To list all the topics in a Kafka cluster, you should use the `kafka-topics.sh` command with the `--list` option. The `--bootstrap-server` option is used to specify the Kafka broker(s) to connect to. The `--zookeeper` option is deprecated in newer versions of Kafka.

## Question 7

**Question:**  
How can you describe the configuration of a topic named "test" using the Kafka CLI?

A. kafka-topics.sh --describe --topic test --zookeeper localhost:2181

B. kafka-topics.sh --describe --topic test --bootstrap-server localhost:9092

C. kafka-configs.sh --describe --entity-type topics --entity-name test --zookeeper localhost:2181

D. kafka-configs.sh --describe --entity-type topics --entity-name test --bootstrap-server localhost:9092

**Answer:** D

To describe the configuration of a topic, you should use the `kafka-configs.sh` command with the `--describe` option. The `--entity-type` option should be set to "topics", and the `--entity-name` option should be set to the name of the topic. The `--bootstrap-server` option is used to specify the Kafka broker(s) to connect to, while `--zookeeper` is deprecated.

**Explanation:**

**Option A**: This is incorrect for two reasons:
1. It uses `kafka-topics.sh` instead of `kafka-configs.sh`
2. It uses the deprecated `--zookeeper` option

**Option B**: While this uses the correct `--bootstrap-server` option, it still uses `kafka-topics.sh` which shows topic metadata but not configuration properties.

**Option C**: This uses the correct tool (`kafka-configs.sh`) but with the deprecated `--zookeeper` option, which should not be used in current Kafka versions.

## Question 8

Which Kafka CLI command is used to produce messages to a topic?

- A. kafka-console-producer.sh
- B. kafka-console-consumer.sh
- C. kafka-topics.sh
- D. kafka-configs.sh

**Answer:** A

**Explanation:**
To produce messages to a topic using the Kafka CLI, you should use the `kafka-console-producer.sh` command. This command reads input from the console and publishes it to the specified Kafka topic. You need to provide the `--bootstrap-server` or `--broker-list` option to specify the Kafka broker(s) to connect to, and the `--topic` option to specify the topic to produce messages to.

## Question 9

How can you consume messages from the beginning of a topic named "test" using the Kafka CLI?

- A. kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
- B. kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test
- C. kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
- D. kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test

**Answer:** A

**Explanation:**
To consume messages from the beginning of a topic using the Kafka CLI, you should use the `kafka-console-consumer.sh` command with the `--from-beginning` option. This option tells the consumer to start consuming from the earliest available offset in the topic. The `--bootstrap-server` option is used to specify the Kafka broker(s) to connect to, and the `--topic` option is used to specify the topic to consume from.

## Question 10

What is the purpose of the `--group` option in the `kafka-console-consumer.sh` command?

- A. To specify the consumer group ID for the console consumer
- B. To specify the number of consumer instances in the group
- C. To specify the list of topics to consume from
- D. To specify the bootstrap server for the consumer

**Answer:** A

**Explanation:**
The `--group` option in the `kafka-console-consumer.sh` command is used to specify the consumer group ID for the console consumer. If not specified, the console consumer will join a random consumer group. Specifying a group ID allows multiple consumer instances to coordinate and distribute the partitions of a topic among themselves for parallel consumption.
