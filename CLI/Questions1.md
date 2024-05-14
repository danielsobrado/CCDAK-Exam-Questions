## Question 2

How can you view the current configuration of a Kafka topic?

A. Use the `kafka-topics.sh --describe` command
B. Use the `kafka-configs.sh --describe` command
C. Use the `zookeeper-shell.sh` command to navigate to the topic's configuration znode
D. Look in the Kafka broker's log files for the topic configuration

**Answer:** A

**Explanation:**
To view the current configuration of a Kafka topic, you can use the `kafka-topics.sh --describe` command.

The `kafka-topics.sh` tool is a command-line utility provided by Kafka for managing topics. The `--describe` option of this tool allows you to retrieve information about a specific topic, including its configuration settings.

When you run `kafka-topics.sh --describe --topic <topic-name>`:

1. The tool connects to a Kafka broker and sends a request to describe the specified topic.
2. The broker retrieves the topic's metadata and configuration from Zookeeper.
3. The tool displays the topic's information, including partitions, replicas, and configuration settings.

The output of the command will show the topic's configuration properties and their values, such as retention policy, cleanup policy, and compression type.

Statement B is incorrect because `kafka-configs.sh --describe` is used to describe broker or client configurations, not topic configurations.

Statement C is incorrect because while topic configurations are stored in Zookeeper, using the `zookeeper-shell.sh` command to navigate the znodes is a low-level approach and not the recommended way to view topic configurations.

Statement D is incorrect because topic configurations are not stored in the Kafka broker's log files. They are stored in Zookeeper and can be accessed through the Kafka tools.
