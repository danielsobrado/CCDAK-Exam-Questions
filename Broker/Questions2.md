## Question 11:
A Kafka cluster is configured with the following settings:
- `default.replication.factor=2`
- `min.insync.replicas=2`

What is the minimum number of brokers required in the cluster to ensure that the cluster can tolerate at least one broker failure without losing the ability to serve write requests?

1. 1
2. 2
3. 3
4. 4

Choose the correct answer.

Response:

The correct answer is **3. 3**.

Explanation:
To tolerate at least one broker failure without losing the ability to serve write requests, the cluster must have enough brokers to satisfy the `min.insync.replicas` requirement even when one broker is down.

With `default.replication.factor=2`, each partition will have two replicas (one leader and one follower). To ensure that writes can succeed even if one broker fails, there must be at least one in-sync replica (ISR) available to acknowledge the write.

Since `min.insync.replicas=2`, a minimum of two replicas (including the leader) must be in-sync for a write to be considered successful. Therefore, the cluster needs at least three brokers to guarantee that there will always be at least two replicas available, even if one broker fails.

If the cluster had only two brokers and one failed, the remaining broker would not be able to satisfy the `min.insync.replicas` requirement, and writes would fail.

## Question 12:
A Kafka cluster has the following configuration:
- `num.partitions=6`
- `default.replication.factor=3`

How many replicas will be created in total across all brokers for a newly created topic that uses the default settings?

1. 6
2. 9
3. 12
4. 18

Choose the correct answer.

Response:

The correct answer is **4. 18**.

Explanation:
The total number of replicas created for a topic is determined by the number of partitions multiplied by the replication factor.

In this case, the cluster is configured with `num.partitions=6`, which means that a newly created topic using the default settings will have 6 partitions.

The `default.replication.factor` is set to 3, indicating that each partition will have 3 replicas (one leader and two followers).

To calculate the total number of replicas, we multiply the number of partitions by the replication factor:
- Total replicas = `num.partitions` × `default.replication.factor`
- Total replicas = 6 × 3 = 18

Therefore, a newly created topic with the default settings will have a total of 18 replicas distributed across the brokers in the cluster.

## Question 13:
A Kafka cluster has the following configuration:
- `unclean.leader.election.enable=false`

What is the implication of this setting when a partition leader fails and there are no in-sync replicas (ISRs) available?

1. The partition will remain unavailable until the failed leader recovers.
2. The partition will elect a new leader from the out-of-sync replicas to maintain availability.
3. The partition will automatically create a new replica to replace the failed leader.
4. The partition will be reassigned to another broker in the cluster.

Choose the correct answer.

Response:

The correct answer is **1. The partition will remain unavailable until the failed leader recovers.**

Explanation:
When `unclean.leader.election.enable` is set to `false`, Kafka enforces a strict consistency model and does not allow the election of a leader from out-of-sync replicas.

In the event of a partition leader failure, Kafka will first attempt to elect a new leader from the set of in-sync replicas (ISRs). ISRs are replicas that are fully caught up with the leader and have all the latest messages.

However, if there are no ISRs available when the leader fails, Kafka has two options:
1. If `unclean.leader.election.enable` is set to `true`, Kafka will elect a new leader from the out-of-sync replicas to maintain availability, potentially resulting in data loss or inconsistency.
2. If `unclean.leader.election.enable` is set to `false`, Kafka will not elect a leader from the out-of-sync replicas and will instead keep the partition unavailable until the failed leader recovers or a new ISR becomes available.

In this scenario, since `unclean.leader.election.enable` is set to `false`, and there are no ISRs available, the partition will remain unavailable until the failed leader recovers. This ensures data consistency but may impact availability until the leader is back online.

## Question 14:
A Kafka broker is configured with the following settings:
- `num.replication.fetchers=4`
- `replica.fetch.max.bytes=1048576`

What is the maximum amount of data that can be fetched by the broker for replication purposes in a single request?

1. 1 MB
2. 4 MB
3. 1048576 bytes
4. 4194304 bytes

Choose the correct answer.

Response:

The correct answer is **1. 1 MB**.

Explanation:
The maximum amount of data that can be fetched by the broker for replication purposes in a single request is determined by the `replica.fetch.max.bytes` configuration.

In this case, `replica.fetch.max.bytes` is set to 1048576, which is equal to 1 MB (1024 * 1024 bytes).

The `num.replication.fetchers` setting specifies the number of fetcher threads used to replicate messages from the leader. However, it does not directly impact the maximum amount of data that can be fetched in a single request.

Each fetcher thread can fetch up to `replica.fetch.max.bytes` of data in a single request. So, even though there are 4 fetcher threads (`num.replication.fetchers=4`), each thread is still limited by the `replica.fetch.max.bytes` value.

Therefore, the maximum amount of data that can be fetched by the broker for replication purposes in a single request is 1 MB, as specified by `replica.fetch.max.bytes`.

## Question 15:
A Kafka cluster is configured with the following settings:
- `log.retention.hours=48`
- `log.retention.bytes=1073741824`
- `log.segment.bytes=536870912`

Assuming a topic has a constant message production rate, which of the following factors will trigger a log segment to be eligible for deletion?

1. The log segment is older than 48 hours.
2. The log segment size exceeds 536870912 bytes (512 MB).
3. The total size of all log segments for the topic exceeds 1073741824 bytes (1 GB).
4. All of the above.

Choose the correct answer.

Response:

The correct answer is **4. All of the above**.

Explanation:
In Kafka, log retention is controlled by a combination of time-based and size-based policies. The configuration properties `log.retention.hours`, `log.retention.bytes`, and `log.segment.bytes` work together to determine when a log segment is eligible for deletion.

1. `log.retention.hours=48`: This setting specifies the maximum time a log segment can be retained before it becomes eligible for deletion. In this case, any log segment older than 48 hours will be eligible for deletion, regardless of its size.

2. `log.segment.bytes=536870912`: This setting determines the maximum size of a single log segment. When a log segment reaches this size (512 MB in this case), Kafka will close the current segment and start a new one. The old segment will be eligible for deletion based on the retention policies.

3. `log.retention.bytes=1073741824`: This setting specifies the maximum total size of all log segments for a topic. If the total size of all log segments exceeds this value (1 GB in this case), Kafka will start deleting the oldest segments to free up space, even if they haven't reached the time-based retention limit.

Therefore, a log segment will be eligible for deletion if any of the following conditions are met:
- The log segment is older than the retention time specified by `log.retention.hours`.
- The log segment size exceeds the size specified by `log.segment.bytes`.
- The total size of all log segments for the topic exceeds the size specified by `log.retention.bytes`.

All three factors independently contribute to the eligibility of a log segment for deletion.

## Question 16

A client connects to a broker in a Kafka cluster and sends a produce request for a topic partition. The broker responds with a 'Not Enough Replicas' error. What does the client do next?

- A. Retries sending the produce request to the same broker
- B. Sends metadata request to the same broker to refresh its metadata
- C. Sends produce request to the controller broker
- D. Sends metadata request to the Zookeeper to find the controller broker

**Answer:** B

**Explanation:**
When a Kafka client receives a 'Not Enough Replicas' error from a broker, it means the broker doesn't have enough in-sync replicas to satisfy the request. The client's next step is to refresh its metadata by sending a metadata request to the same broker. This will provide the client with the most up-to-date information about the cluster, including the current leader for the partition.

- A is incorrect because retrying the same request without refreshing metadata is likely to result in the same error.
- C is incorrect because the client doesn't directly send requests to the controller broker.
- D is incorrect because the client communicates with Zookeeper only for the initial bootstrap, not for regular operations.

## Question 17

A Kafka consumer is consuming from a topic partition. It sends a fetch request to the broker and receives a 'Replica Not Available' error. What is the consumer's next action?

- A. Backs off and retries the fetch request after a short delay
- B. Sends an offset commit request to trigger partition rebalancing
- C. Sends a metadata request to refresh its view of the cluster
- D. Closes the connection and tries connecting to a different broker

**Answer:** C

**Explanation:**
When a consumer receives a 'Replica Not Available' error, it means the broker it's connected to doesn't have a replica of the partition available to serve the request. The consumer's next step is to send a metadata request to refresh its view of the cluster. This will provide updated information about which brokers are currently hosting the partition replicas.

- A is incorrect because simply retrying after a delay may not resolve the issue if the consumer's metadata is stale.
- B is incorrect because committing offsets is not directly related to handling this error and doesn't trigger rebalancing.
- D is incorrect because closing the connection is not necessary. The consumer can refresh metadata over the existing connection.

## Question 18

What happens if you produce to a topic that does not exist, and the broker setting `auto.create.topics.enable` is set to `false`?

- A. The broker will create the topic with default configurations
- B. The broker will reject the produce request and the producer will throw an exception
- C. The producer will automatically create the topic
- D. The producer will wait until the topic is created

**Answer:** B

**Explanation:**
When `auto.create.topics.enable` is set to `false` on the Kafka brokers, they will not automatically create a topic if a producer tries to produce to a non-existent topic. Instead, the broker will reject the produce request, and the producer will throw a `TopicExistsException`.

- A is incorrect because the broker will not create the topic when `auto.create.topics.enable` is `false`.
- C is incorrect because the producer does not have the ability to create topics, only the broker does.
- D is incorrect because the producer will not wait, it will immediately throw an exception.

## Question 19

What is the default value of `auto.create.topics.enable` in Kafka?

- A. `true`
- B. `false`
- C. It is not set by default
- D. It depends on the Kafka version

**Answer:** A

**Explanation:**
In Kafka, `auto.create.topics.enable` is set to `true` by default. This means that by default, when a producer tries to produce to a non-existent topic or a consumer tries to consume from a non-existent topic, Kafka will automatically create the topic with default configurations.

- B is incorrect because `false` is not the default value.
- C is incorrect because the property does have a default value.
- D is incorrect because the default value is consistent across Kafka versions.

## Question 20

When a topic is automatically created due to `auto.create.topics.enable` being `true`, what configurations are used for the new topic?

- A. The configurations specified by the producer or consumer
- B. The default configurations set on the broker
- C. A combination of producer/consumer configurations and broker defaults
- D. No configurations are set, the topic is created with empty configuration

**Answer:** B

**Explanation:**
When Kafka automatically creates a topic due to `auto.create.topics.enable` being `true`, it uses the default topic configurations set on the broker. These defaults are defined by the following broker settings:

- `num.partitions`: The default number of partitions for automatically created topics.
- `default.replication.factor`: The default replication factor for automatically created topics.

Any topic-level configurations set by the producer or consumer are ignored during automatic topic creation.

- A and C are incorrect because the producer/consumer configurations are not used for automatic topic creation.
- D is incorrect because the topic is not created with empty configuration, but with the broker's default configurations.
