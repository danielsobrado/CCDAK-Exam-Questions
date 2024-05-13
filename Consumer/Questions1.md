## Question 1

Which of the following is stored in the Kafka `__consumer_offsets` topic? (Select two)

A. The latest committed offset for each consumer group
B. The list of consumers in each consumer group
C. The mapping of partitions to consumer groups
D. The last produced message for each topic partition
E. The earliest committed offset for each consumer group

**Answer:** A, B

**Explanation:**
The `__consumer_offsets` topic in Kafka is used to store consumer group metadata, including:

- The latest committed offset for each consumer group, so consumers can resume from where they left off.
- The list of consumers currently active in each consumer group, to facilitate group management.

The other options are not stored in this topic:

- C: The mapping of partitions to consumer groups is managed by the Kafka coordinator, not stored in this topic.
- D: The last produced message for each partition is not related to consumer offsets and is not stored here.
- E: Only the latest offset is stored, not the earliest.

## Question 2

There are two consumers C1 and C2 belonging to the same group G subscribed to topics T1, T2, and T3. Each topic has 4 partitions. Assuming all partitions have data, how many partitions will each consumer be assigned with the Range Assignor?

A. C1: 6 partitions, C2: 6 partitions
B. C1: 4 partitions, C2: 8 partitions
C. C1: 2 partitions from each topic, C2: 2 partitions from each topic
D. C1: 1 partition from each topic, C2: 3 partitions from each topic

**Answer:** A

**Explanation:**
With the Range Assignor, each consumer will be assigned a contiguous range of partitions from each topic. In this case, with 4 partitions per topic and 2 consumers, each consumer will get 2 partitions from each topic.

## Question 3

There are four consumers C1, C2, C3, C4 belonging to the same group G subscribed to two topics T1 and T2. T1 has 3 partitions and T2 has 2 partitions. With the Round Robin Assignor, which consumer(s) will be assigned partition 2 from topic T1?

A. C1
B. C2
C. C3
D. C4

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

A. All partitions will be reassigned evenly among C2 and C3
B. C2 and C3 will retain their existing partitions, and the partitions from C1 will be reassigned to either C2 or C3
C. All partitions will be reassigned randomly to C2 and C3
D. C2 and C3 will retain their existing partitions, and the partitions from C1 will not be reassigned

**Answer:** B

**Explanation:**
The Sticky Assignor aims to minimize partition movement when the group membership changes. When a consumer leaves, it tries to reassign the partitions from the leaving consumer to the remaining consumers, while keeping the existing assignments as sticky as possible.

- A, C are not correct because they involve unnecessary partition movement.
- D is incorrect because the partitions from the leaving consumer will be reassigned, not left unassigned.

## Question 5

A Kafka Streams application tries to consume from an input topic partition. It receives an 'Offset Out Of Range' error from the broker. How should the application handle this?

A. Reset the consumer offset to the earliest offset and retry
B. Reset the consumer offset to the latest offset and retry
C. Trigger a shutdown of the Streams application
D. Ignore the error and continue processing other partitions

**Answer:** A

**Explanation:**
An 'Offset Out Of Range' error in Kafka Streams indicates that the application is trying to fetch from an offset that is no longer available in the partition, usually because the data has been deleted due to retention policies. The recommended way to handle this is to reset the consumer offset to the earliest available offset and retry consuming from there.

- B is not recommended because resetting to the latest offset will skip over the missing data.
- C is too extreme. The error can be handled without shutting down the entire application.
- D will lead to data loss as the partition with the error will be ignored.

## Question 6

You are designing a Kafka consumer application that will consume messages from a topic. The messages in the topic are in JSON format. Which of the following properties should you set in the consumer configuration?

A. `key.deserializer=JsonDeserializer`
B. `value.deserializer=JsonDeserializer`
C. `key.deserializer=StringDeserializer`
D. `value.deserializer=StringDeserializer`

**Answer:** B

**Explanation:**
In a Kafka consumer application, you need to specify how to deserialize the message keys and values. Since the messages in the topic are in JSON format, you should set:

- `value.deserializer=JsonDeserializer`: This tells the consumer to use the `JsonDeserializer` to deserialize the message values from JSON to Java objects.

The other options are not correct:

- A: `key.deserializer=JsonDeserializer` would be correct if the message keys were also in JSON format. However, the question doesn't specify the key format.
- C and D: `StringDeserializer` is not appropriate because the message values are in JSON format, not plain strings.
