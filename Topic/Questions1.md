## Question 1

Which of the following statements about `acks` and `min.insync.replicas` are true? (Select all that apply)

A. `acks` is a producer configuration, while `min.insync.replicas` is a topic configuration
B. `acks` and `min.insync.replicas` are both producer configurations
C. `acks` and `min.insync.replicas` are both topic configurations
D. `acks=all` and `min.insync.replicas=1` provides the strongest durability guarantee
E. `acks=1` and `min.insync.replicas=2` provides the strongest durability guarantee
F. For `acks=all` to provide any additional durability over `acks=1`, `min.insync.replicas` must be greater than 1

**Answer:** A, F

**Explanation:**
`acks` and `min.insync.replicas` are two crucial configurations in Kafka that work together to control the durability of writes:

- `acks` is a producer configuration that specifies how many acknowledgments the producer requires the leader to have received before considering a write successful.
- `min.insync.replicas` is a topic-level configuration (which can also be set as a broker default) that specifies the minimum number of replicas that must acknowledge a write for the write to be considered successful.

For `acks=all` to provide any additional durability guarantee over `acks=1`, `min.insync.replicas` must be set to a value greater than 1. If `min.insync.replicas=1`, then `acks=all` and `acks=1` are effectively equivalent, as the leader will acknowledge the write as soon as it has been written to its own log, regardless of the state of the followers.

- B and C are incorrect because `acks` and `min.insync.replicas` are configurations at different levels (producer and topic/broker, respectively).
- D is incorrect because `acks=all` with `min.insync.replicas=1` is no stronger than `acks=1`.
- E is incorrect because `acks=1` does not interact with `min.insync.replicas` at all, so this combination does not provide the strongest durability guarantee.

## Question 2

What is the relationship between `unclean.leader.election.enable` and `min.insync.replicas`?

A. They control the same thing and should always be set to the same value
B. `unclean.leader.election.enable` overrides `min.insync.replicas`
C. They are independent and one does not affect the other
D. If `unclean.leader.election.enable=true`, `min.insync.replicas` can be violated during leader election

**Answer:** D

**Explanation:**
`unclean.leader.election.enable` and `min.insync.replicas` are two Kafka configurations that can interact in certain scenarios:

- `unclean.leader.election.enable` is a broker configuration that controls whether a replica that is out of sync with the leader can be elected as the new leader if the existing leader fails.
- `min.insync.replicas` is a topic-level configuration that specifies the minimum number of replicas that must be in-sync with the leader for writes to succeed.

Normally, a replica that is not fully in-sync with the leader cannot be elected as the new leader. This protects against data loss, as an out-of-sync replica may be missing some of the latest messages.

However, if `unclean.leader.election.enable` is set to `true`, this protection is disabled. In this case, if all the in-sync replicas fail and only out-of-sync replicas remain, one of those out-of-sync replicas can be elected as the new leader. This can violate `min.insync.replicas` and potentially lead to data loss, but it allows the partition to remain available.

- A and C are incorrect because the two configurations do not control the same thing and they are not completely independent.
- B is incorrect because `unclean.leader.election.enable` does not override `min.insync.replicas`, it just allows it to be violated in certain edge cases.

## Question 3

A Kafka cluster has 3 brokers. You create a topic with 6 partitions and 2 consumers in a consumer group subscribed to this topic. What is the maximum number of partitions that can be assigned to a single consumer?

A. 1
B. 2
C. 3
D. 6

**Answer:** D

**Explanation:**
In Kafka, the number of partitions assigned to each consumer in a consumer group depends on the total number of partitions and the number of consumers. Kafka's goal is to evenly distribute partitions among consumers, but if there are more partitions than consumers, some consumers will necessarily handle more partitions.

In this case, with 6 partitions and 2 consumers, the maximum number of partitions that can be assigned to a single consumer is 6. This would happen if one consumer is assigned 4 partitions and the other is assigned 2 partitions.

It's important to note that having more partitions than consumers is a valid and common configuration. It allows for adding more consumers later to scale out consumption throughput.

- A, B, and C are incorrect because they do not represent the maximum possible assignment. With 6 partitions and 2 consumers, it's possible for a consumer to be assigned more than 3 partitions.

## Question 4

A topic has 10 partitions and a replication factor of 3. There are 2 consumers in a consumer group subscribed to this topic. The cluster has 5 brokers. How would the partitions be assigned to the consumers to achieve maximum throughput?

A. Consumer 1: Partitions 0-4, Consumer 2: Partitions 5-9
B. Consumer 1: Partitions 0, 2, 4, 6, 8, Consumer 2: Partitions 1, 3, 5, 7, 9
C. Consumer 1: Partitions 0, 1, 2, Consumer 2: Partitions 3, 4, 5, Unassigned: 6, 7, 8, 9
D. Consumer 1: Partitions 0-9

**Answer:** A

**Explanation:**
To achieve maximum throughput, Kafka aims to evenly distribute the partitions among the available consumers in a consumer group. This allows for parallel consumption of data.

In this case, with 10 partitions and 2 consumers, the optimal distribution for maximum throughput is:

- Consumer 1: Partitions 0, 1, 2, 3, 4
- Consumer 2: Partitions 5, 6, 7, 8, 9

This way, each consumer handles 5 partitions, and all partitions are being consumed concurrently.

The replication factor and the number of brokers do not directly affect the partition assignment to consumers. The replication factor is about data durability and the number of brokers is about the cluster's capacity, but the consumer-partition assignment is handled independently by the consumer group coordinator.

- B is a valid distribution but does not provide any additional throughput compared to A.
- C is suboptimal because it leaves some partitions unassigned, reducing total throughput.
- D is incorrect because it assigns all partitions to a single consumer, eliminating the benefits of parallel consumption.
