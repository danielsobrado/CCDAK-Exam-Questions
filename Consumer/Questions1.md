## Question 1

Which of the following is stored in the Kafka `__consumer_offsets` topic? (Select two)

- A. The latest committed offset for each consumer group
- B. The list of consumers in each consumer group
- C. The mapping of partitions to consumer groups
- D. The last produced message for each topic partition
- E. The earliest committed offset for each consumer group

**Answer:** A, C

**Explanation:**
The `__consumer_offsets` topic in Kafka stores:

A. The latest committed offset for each consumer group: This allows consumers to resume consumption from the correct point after restarts or failures.
C. The mapping of partitions to consumer groups: The keys in the __consumer_offsets topic records include the Group ID, Topic, and Partition, effectively mapping consumer groups to the partitions they are consuming.
The other options are not stored in this topic:

B. The list of consumers in each consumer group: Managed by the Group Coordinator and not stored in __consumer_offsets.
D. The last produced message for each topic partition: Stored in the topic partitions themselves.
E. The earliest committed offset for each consumer group: Only the latest committed offsets are stored.

## Question 2

There are two consumers C1 and C2 belonging to the same group G subscribed to topics T1, T2, and T3. Each topic has 4 partitions. Assuming all partitions have data, how many partitions will each consumer be assigned with the Range Assignor?

- A. C1: 6 partitions, C2: 6 partitions
- B. C1: 4 partitions, C2: 8 partitions
- C. C1: 2 partitions from each topic, C2: 2 partitions from each topic
- D. C1: 1 partition from each topic, C2: 3 partitions from each topic

**Answer:** A

**Explanation:**
With the Range Assignor, each consumer will be assigned a contiguous range of partitions from each topic. In this case, with 4 partitions per topic and 2 consumers, each consumer will get 2 partitions from each topic.

## Question 3

There are four consumers C1, C2, C3, C4 belonging to the same group G subscribed to two topics T1 and T2. T1 has 3 partitions and T2 has 2 partitions. With the Round Robin Assignor, which consumer(s) will be assigned partition 2 from topic T1?

- A. C1
- B. C2
- C. C3
- D. C4

**Answer:** C

**Explanation:**
With the Round Robin Assignor, partitions are assigned to consumers sequentially, one by one, going around all the consumers repeatedly. In this case, the assignment will be:

- C1: T1-0, T2-1
- C2: T1-1, T2-0
- C3: T1-2
- C4: (no partitions)

So partition 2 from topic T1 will be assigned to consumer C3.

## Question 4

There are three consumers C1, C2, C3 belonging to the same group G subscribed to a topic T. The topic has 10 partitions. If the Sticky Assignor is used, and C1 leaves the group, how will the partitions be rebalanced?

- A. All partitions will be reassigned evenly among C2 and C3
- B. C2 and C3 will retain their existing partitions, and the partitions from C1 will be reassigned to either C2 or C3
- C. All partitions will be reassigned randomly to C2 and C3
- D. C2 and C3 will retain their existing partitions, and the partitions from C1 will not be reassigned

**Answer:** B

**Explanation:**
The Sticky Assignor aims to minimize partition movement when the group membership changes. When a consumer leaves, it tries to reassign the partitions from the leaving consumer to the remaining consumers, while keeping the existing assignments as sticky as possible.

- A, C are not correct because they involve unnecessary partition movement.
- D is incorrect because the partitions from the leaving consumer will be reassigned, not left unassigned.

## Question 5

A Kafka Streams application tries to consume from an input topic partition. It receives an 'Offset Out Of Range' error from the broker. How should the application handle this?

- A. Reset the consumer offset to the earliest offset and retry
- B. Reset the consumer offset to the latest offset and retry
- C. Trigger a shutdown of the Streams application
- D. Ignore the error and continue processing other partitions

**Answer:** A

**Explanation:**
- A. 'Offset Out Of Range' error in Kafka Streams indicates that the application is trying to fetch from an offset that is no longer available in the partition, usually because the data has been deleted due to retention policies. The recommended way to handle this is to reset the consumer offset to the earliest available offset and retry consuming from there.

- B is not recommended because resetting to the latest offset will skip over the missing data.
- C is too extreme. The error can be handled without shutting down the entire application.
- D will lead to data loss as the partition with the error will be ignored.

## Question 6

You are designing a Kafka consumer application that will consume messages from a topic. The messages in the topic are in JSON format. Which of the following properties should you set in the consumer configuration?

- A. `key.deserializer=JsonDeserializer`
- B. `value.deserializer=JsonDeserializer`
- C. `key.deserializer=StringDeserializer`
- D. `value.deserializer=StringDeserializer`

**Answer:** B

**Explanation:**
In a Kafka consumer application, you need to specify how to deserialize the message keys and values. Since the messages in the topic are in JSON format, you should set:

- `value.deserializer=JsonDeserializer`: This tells the consumer to use the `JsonDeserializer` to deserialize the message values from JSON to Java objects.

The other options are not correct:

- A: `key.deserializer=JsonDeserializer` would be correct if the message keys were also in JSON format. However, the question doesn't specify the key format.
- C and D: `StringDeserializer` is not appropriate because the message values are in JSON format, not plain strings.

## Question 7

A consumer wants to read messages from a specific partition of a topic. Which of the following methods should be used?

- A. `KafkaConsumer.subscribe(String topic, int partition)`
- B. `KafkaConsumer.assign(Collection<TopicPartition> partitions)`
- C. `KafkaConsumer.subscribe(Collection<TopicPartition> partitions)`
- D. `KafkaConsumer.assign(String topic, int partition)`

**Answer:** B

**Explanation:**
To read messages from a specific partition of a topic, a consumer should use the `assign` method of the `KafkaConsumer` class.

The `assign` method takes a collection of `TopicPartition` objects as a parameter. Each `TopicPartition` represents a specific partition of a topic. By passing a collection of `TopicPartition` objects to `assign`, the consumer is explicitly assigned to those specific partitions.

The other options are incorrect:

- A and D are incorrect because `KafkaConsumer` does not have a method that takes a topic and partition as separate parameters.
- C is incorrect because `subscribe` is used for subscribing to entire topics, not specific partitions. When you subscribe to a topic, Kafka automatically assigns partitions to the consumer.

Using `assign` allows for fine-grained control over which partitions a consumer reads from. It's useful in scenarios where you want to manually balance partitions across consumers or implement a custom partition assignment strategy.

## Question 8

What happens when a consumer is assigned a partition that does not exist in the Kafka cluster?

- A. The consumer will ignore the non-existent partition and continue processing other assigned partitions
- B. The consumer will throw an exception and stop processing
- C. The consumer will create the partition automatically
- D. The consumer will wait until the partition is created

**Answer:** B

**Explanation:**
When a consumer is assigned a partition that does not exist in the Kafka cluster, it will throw an exception and stop processing.

In Kafka, partitions are created administratively before data is produced to them. Consumers do not have the ability to create partitions automatically. If a consumer tries to read from a non-existent partition, it is considered an error condition.

When a consumer encounters a non-existent partition in its assignment:

- It will throw a `InvalidTopicException` or `UnknownTopicOrPartitionException`.
- The consumer will stop processing and will not continue reading from other assigned partitions.
- The application will need to handle the exception and decide how to proceed (e.g., logging an error, retrying with a valid assignment, etc.).

Therefore, statements A, C, and D are incorrect. The consumer will not ignore the non-existent partition, create it automatically, or wait for it to be created. It will throw an exception and stop processing.

To avoid this error, ensure that the partitions assigned to a consumer actually exist in the Kafka cluster. Double-check the topic names and partition numbers in your consumer configuration or application code.

## Question 9

Can a consumer dynamically change the partitions it is assigned to without stopping and restarting?

- A. Yes, by calling `KafkaConsumer.subscribe()` with a new set of topics
- B. Yes, by calling `KafkaConsumer.assign()` with a new set of partitions
- C. No, partition assignment can only be changed when the consumer is first started
- D. No, partition assignment is fixed for the entire lifecycle of the consumer

**Answer:** B

**Explanation:**
A Kafka consumer can dynamically change the partitions it is assigned to without stopping and restarting by calling the `KafkaConsumer.assign()` method with a new set of partitions.

The `assign` method allows a consumer to explicitly specify which partitions it should consume from. By calling `assign` with a different set of partitions, the consumer can dynamically change its assignment.

Here's how it works:

1. The consumer calls `assign` with a new collection of `TopicPartition` objects representing the desired partitions to consume from.
2. Kafka updates the consumer's assignment to the specified partitions.
3. The consumer will stop consuming from its previous assignment and start consuming from the newly assigned partitions.
4. The consumer can continue processing messages from the new partitions without needing to restart.

This dynamic partition assignment is useful in scenarios where you want to implement custom partition load balancing, respond to partition rebalances, or adjust consumer workload at runtime.

Statement A is incorrect because `subscribe` is used for subscribing to entire topics, not changing partition assignments. When you call `subscribe`, Kafka will automatically assign partitions to the consumer based on the configured partition assignment strategy.

Statements C and D are incorrect because partition assignment is not fixed for the entire lifecycle of a consumer. It can be changed dynamically using the `assign` method.

## Question 10

A consumer is part of a consumer group and is currently processing messages. If the consumer crashes and is restarted, what will happen?

- A. The consumer will resume processing from the last committed offset
- B. The consumer will start processing from the earliest available offset
- C. The consumer will start processing from the latest available offset
- D. The consumer will be assigned a new set of partitions

**Answer:** A

**Explanation:**
When a consumer in a consumer group crashes and is restarted, it will resume processing from the last committed offset.

In Kafka, each consumer in a consumer group maintains its own offset position for each partition it is assigned to. Periodically, the consumer commits its offsets to Kafka to mark its progress. If a consumer crashes or is shut down, its offsets remain committed in Kafka.

When the consumer is restarted:

1. It will rejoin the consumer group.
2. Kafka will reassign partitions to the consumers in the group, including the restarted consumer.
3. For each assigned partition, the consumer will resume processing from the last committed offset.

This behavior ensures that the consumer does not miss any messages and avoids duplicating message processing.

Statement B is incorrect because the consumer will not start from the earliest available offset unless it is explicitly configured to do so (e.g., by setting `auto.offset.reset=earliest`).

Statement C is incorrect because the consumer will not start from the latest available offset unless it is explicitly configured to do so (e.g., by setting `auto.offset.reset=latest`).

Statement D is incorrect because the consumer will not necessarily be assigned a new set of partitions. Kafka will reassign partitions based on the consumer group's partition assignment strategy, which may or may not result in the same assignments as before.
