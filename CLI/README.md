## Kafka CLI Tools Overview

Kafka provides a set of command-line tools that allow administrators and developers to interact with the Kafka cluster, topics, consumer groups, and other Kafka entities. These tools are fundamental for tasks such as creating topics, producing and consuming messages, and monitoring consumer group offsets.

### Key CLI Tools

- **kafka-topics.sh**: Used for creating, deleting, describing, or altering topics on a Kafka broker.
  ```
  # Create a topic
  kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic my-topic
  ```
- **kafka-console-producer.sh**: A utility that allows you to produce messages to a Kafka topic from the command line.
  ```
  # Produce a message
  kafka-console-producer.sh --broker-list localhost:9092 --topic my-topic
  ```
- **kafka-console-consumer.sh**: Enables consuming messages from a Kafka topic and display them to standard output.
  ```
  # Consume messages
  kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning
  ```
- **kafka-consumer-groups.sh**: Offers a way to manage consumer groups, including listing groups, describing group details, and resetting consumer group offsets.
  ```
  # Describe consumer group
  kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-group
  ```
- **kafka-acls.sh**: Used for managing Access Control Lists (ACLs) for Kafka.
- **kafka-configs.sh**: Allows for modifying broker, topic, and client configs.
- **kafka-mirror-maker.sh**: A tool for mirroring data between Kafka clusters.

| Command | Explanation |
|---------|-------------|
| **kafka-topics.sh** | A tool for managing Kafka topics. |
| --bootstrap-server | Specifies a comma-separated list of Kafka broker addresses. This is the recommended option, replacing `--zookeeper`. |
| --create | Creates a new topic. |
| --delete | Deletes an existing topic. |
| --alter | Alters the configuration of an existing topic. |
| --list | Lists all available topics. |
| --describe | Describes the details of a specific topic, including partitions and replication factor. |
| --if-exists | Used with `--delete` or `--alter` to perform the operation only if the topic exists. |
| --if-not-exists | Used with `--create` to create the topic only if it does not already exist. |
| --topic | Specifies the name of the topic to create, modify, or delete. |
| --partitions | Specifies the number of partitions for the topic. |
| --replication-factor | Specifies the replication factor for each partition in the topic. |
| --config | Specifies topic-level configuration properties. |
| **kafka-console-producer.sh** | A command-line tool that allows you to produce messages to a Kafka topic. |
| --bootstrap-server | Specifies a comma-separated list of Kafka broker addresses. |
| --topic | Specifies the name of the topic to produce messages to. |
| --producer-property | Specifies a producer configuration property. |
| **kafka-console-consumer.sh** | A command-line tool that allows you to consume messages from a Kafka topic. |
| --bootstrap-server | Specifies a comma-separated list of Kafka broker addresses. |
| --topic | Specifies the name of the topic to consume messages from. |
| --from-beginning | Starts consuming messages from the beginning of the topic. |
| --group | Specifies the consumer group ID for the consumer. |
| --consumer-property | Specifies a consumer configuration property. |
| **kafka-consumer-groups.sh** | A tool for managing Kafka consumer groups. |
| --bootstrap-server | Specifies a comma-separated list of Kafka broker addresses. This is a required flag. |
| --group | Specifies the ID of the consumer group to describe or manage. This is a required flag for most operations. |
| --describe | Describes consumer group details, including the current offset and lag for each partition. |
| --list | Lists all consumer groups. |
| --reset-offsets | Resets the offset for a consumer group. |
| --to-earliest | Used with `--reset-offsets` to reset the offset to the earliest message in each partition. |
| --to-latest | Used with `--reset-offsets` to reset the offset to the latest message in each partition. |
| --to-offset | Used with `--reset-offsets` to reset the offset to a specific value for each partition. |
| --shift-by | Used with `--reset-offsets` to shift the offset by a relative value for each partition. |
| --execute | Executes the offset reset operation. Without this flag, the reset operation is only displayed but not executed. |
| --all-topics | Used with `--describe` to describe the group state for all topics. |
| --all-groups | Used with `--list` to list all consumer groups. |

Note: While ZooKeeper is still a valid option for managing Kafka clusters, it's no longer the most up-to-date approach. The Kafka community is moving towards the self-managed KRaft mode, which simplifies the architecture and improves scalability.

### Best Practices

- **Security**: When working with CLI tools in a production environment, always consider security implications, especially when configuring ACLs or SSL/TLS.
- **Performance Monitoring**: Use CLI tools like `kafka-consumer-groups.sh` for monitoring consumer lag and ensuring that your consumers are performing as expected.
- **Configuration Management**: Use `kafka-configs.sh` to adjust and fine-tune topic and broker configurations to optimize performance and resource usage.

### Troubleshooting

- **Consumer Group Issues**: Utilize `kafka-consumer-groups.sh` to identify and resolve issues related to consumer group lag and partition assignment.
- **Topic Configuration**: Modify topic configurations dynamically using `kafka-configs.sh` to address performance bottlenecks.

### Useful Commands

Here are a few commands that are particularly useful for managing a Kafka environment:

- Listing topics:
  ```
  kafka-topics.sh --list --bootstrap-server localhost:9092
  ```
- Describing topic details:
  ```
  kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic my-topic
  ```
- Resetting consumer group offsets:
  ```
  kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-group --reset-offsets --to-earliest --execute --topic my-topic
  ```
