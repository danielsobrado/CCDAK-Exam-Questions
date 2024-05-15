## Question 11

How can you delete a topic named "test" using the Kafka CLI?

- A. kafka-topics.sh --delete --topic test --zookeeper localhost:2181
- B. kafka-topics.sh --delete --topic test --bootstrap-server localhost:9092
- C. kafka-configs.sh --delete --entity-type topics --entity-name test --bootstrap-server localhost:9092
- D. kafka-console-producer.sh --delete --topic test --bootstrap-server localhost:9092

**Answer:** B

**Explanation:**
To delete a topic using the Kafka CLI, you should use the `kafka-topics.sh` command with the `--delete` option. The `--bootstrap-server` option specifies the Kafka broker(s) to connect to.

## Question 12

What command can you use to reset offsets for a consumer group in Kafka?

- A. kafka-consumer-groups.sh --reset-offsets
- B. kafka-topics.sh --reset-offsets
- C. kafka-console-consumer.sh --reset-offsets
- D. kafka-configs.sh --reset-offsets

**Answer:** A

**Explanation:**
To reset offsets for a consumer group, you should use the `kafka-consumer-groups.sh` command with the `--reset-offsets` option.

## Question 13

How can you check the consumer group lag for a specific consumer group using the Kafka CLI?

- A. kafka-consumer-groups.sh --describe --group <group-id> --bootstrap-server localhost:9092
- B. kafka-topics.sh --describe --group <group-id> --bootstrap-server localhost:9092
- C. kafka-configs.sh --describe --group <group-id> --bootstrap-server localhost:9092
- D. kafka-console-consumer.sh --describe --group <group-id> --bootstrap-server localhost:9092

**Answer:** A

**Explanation:**
To check the consumer group lag, you should use the `kafka-consumer-groups.sh` command with the `--describe` option and specify the `--group` option along with the consumer group ID.

## Question 14

Which command is used to increase the number of partitions for an existing topic?

- A. kafka-topics.sh --alter --topic <topic-name> --partitions <number-of-partitions> --bootstrap-server localhost:9092
- B. kafka-topics.sh --create --topic <topic-name> --partitions <number-of-partitions> --bootstrap-server localhost:9092
- C. kafka-configs.sh --alter --entity-type topics --entity-name <topic-name> --partitions <number-of-partitions> --bootstrap-server localhost:9092
- D. kafka-console-producer.sh --alter --topic <topic-name> --partitions <number-of-partitions> --bootstrap-server localhost:9092

**Answer:** A

**Explanation:**
To increase the number of partitions for an existing topic, you should use the `kafka-topics.sh` command with the `--alter` option and specify the `--partitions` option with the new number of partitions.

## Question 15

How can you view the log of a specific Kafka topic?

- A. kafka-log-dirs.sh --describe --topic <topic-name> --bootstrap-server localhost:9092
- B. kafka-console-consumer.sh --topic <topic-name> --from-beginning --bootstrap-server localhost:9092
- C. kafka-topics.sh --describe --topic <topic-name> --bootstrap-server localhost:9092
- D. kafka-console-producer.sh --log --topic <topic-name> --bootstrap-server localhost:9092

**Answer:** B

**Explanation:**
To view the log of a specific Kafka topic, you can use the `kafka-console-consumer.sh` command with the `--from-beginning` option to start consuming messages from the earliest available offset in the topic.

## Question 16

Which command can be used to change the configuration of a Kafka broker?

- A. kafka-configs.sh --alter --entity-type brokers --entity-name <broker-id> --add-config <key>=<value> --bootstrap-server localhost:9092
- B. kafka-configs.sh --alter --entity-type brokers --entity-name <broker-id> --add-config <key>=<value> --zookeeper localhost:2181
- C. kafka-topics.sh --alter --entity-type brokers --entity-name <broker-id> --add-config <key>=<value> --bootstrap-server localhost:9092
- D. kafka-topics.sh --alter --entity-type brokers --entity-name <broker-id> --add-config <key>=<value> --zookeeper localhost:2181

**Answer:** A

**Explanation:**
To change the configuration of a Kafka broker, you should use the `kafka-configs.sh` command with the `--alter` option, specifying the `--entity-type` as brokers and the `--entity-name` as the broker ID. The `--bootstrap-server` option specifies the Kafka broker(s) to connect to.

## Question 17

How can you reassign partitions in a Kafka cluster?

- A. kafka-reassign-partitions.sh --execute --reassignment-json-file <file-path> --bootstrap-server localhost:9092
- B. kafka-topics.sh --execute --reassignment-json-file <file-path> --bootstrap-server localhost:9092
- C. kafka-console-producer.sh --execute --reassignment-json-file <file-path> --bootstrap-server localhost:9092
- D. kafka-configs.sh --execute --reassignment-json-file <file-path> --bootstrap-server localhost:9092

**Answer:** A

**Explanation:**
To reassign partitions in a Kafka cluster, you should use the `kafka-reassign-partitions.sh` command with the `--execute` option and provide the path to the reassignment JSON file.

## Question 18

Which command can be used to create a consumer group in Kafka?

- A. kafka-console-consumer.sh --create-group --group <group-id> --bootstrap-server localhost:9092
- B. kafka-consumer-groups.sh --create --group <group-id> --bootstrap-server localhost:9092
- C. kafka-topics.sh --create-group --group <group-id> --bootstrap-server localhost:9092
- D. Kafka consumer groups are created automatically when a consumer joins the group for the first time.

**Answer:** D

**Explanation:**
Kafka consumer groups are created automatically when a consumer joins the group for the first time. There is no specific CLI command to create a consumer group manually.

## Question 19

How can you move messages from one Kafka topic to another using the CLI?

- A. kafka-reassign-partitions.sh --source-topic <source-topic> --destination-topic <destination-topic> --bootstrap-server localhost:9092
- B. Use a combination of kafka-console-consumer.sh and kafka-console-producer.sh
- C. kafka-topics.sh --move --source-topic <source-topic> --destination-topic <destination-topic> --bootstrap-server localhost:9092
- D. kafka-console-producer.sh --move --source-topic <source-topic> --destination-topic <destination-topic> --bootstrap-server localhost:9092

**Answer:** B

**Explanation:**
To move messages from one Kafka topic to another using the CLI, you can use a combination of `kafka-console-consumer.sh` to consume messages from the source topic and `kafka-console-producer.sh` to produce them to the destination topic.

## Question 20

What is the purpose of the `--offset` option in the `kafka-console-consumer.sh` command?

- A. To specify the starting offset for consuming messages
- B. To specify the offset at which messages should be deleted
- C. To specify the offset at which messages should be produced
- D. To reset the offset for a consumer group

**Answer:** A

**Explanation:**
The `--offset` option in the `kafka-console-consumer.sh` command is used to specify the starting offset for consuming messages. This allows you to start consuming from a specific point in the topic.
