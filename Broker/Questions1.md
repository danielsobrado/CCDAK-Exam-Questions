Question 1:
Assuming a Kafka topic is configured with the following settings:
- `log.segment.bytes` = 1073741824 (1GB)
- `log.retention.ms` = 86400000 (1 day)
- `log.retention.bytes` = -1

Which of the following statements accurately describes the log retention policy for this Kafka topic?

1. Logs are retained based on size; once the log size exceeds 1GB, older segments are deleted.
2. Logs are retained for exactly one day, regardless of the size of the log.
3. Logs are retained until the size of the log exceeds 1GB or for one day, whichever comes first.
4. Logs are retained indefinitely, as `log.retention.bytes` is set to -1, overriding other retention configurations.

Choose the correct answer.

Response:

The correct answer is **2. Logs are retained for exactly one day, regardless of the size of the log.**

Explanation:

1. **Incorrect**. Although `log.segment.bytes` is set to 1GB, this setting alone does not dictate log retention based on size. It specifies the maximum size of a single log segment file. The deletion policy based on size is controlled by `log.retention.bytes`, which is not effectively set here due to its value being -1 (indicating no limit).

2. **Correct**. The setting `log.retention.ms` = 86400000 specifies that logs are retained for 86400000 milliseconds, which is equivalent to 24 hours or one day. This means logs are deleted after one day, regardless of their size, as long as `log.retention.bytes` does not impose a stricter limit, which in this case, it does not (`log.retention.bytes` = -1).

3. **Incorrect**. This statement misinterprets the settings. Kafka does not use both `log.retention.ms` and `log.retention.bytes` to determine a "whichever comes first" policy. Instead, both conditions must be met for a log segment to be eligible for deletion. Given `log.retention.bytes` = -1, size-based deletion is effectively disabled, leaving time-based deletion as the operative policy.

4. **Incorrect**. The statement that logs are retained indefinitely is wrong because `log.retention.ms` is explicitly set to a finite duration (86400000 ms or 1 day), dictating a time-based retention policy. The `-1` value for `log.retention.bytes` means there is no size limit on log retention, but it does not affect or override the time-based retention set by `log.retention.ms`.

Question 2:
Consider a Kafka topic with the following configuration:
- `cleanup.policy` = "compact,delete"
- `min.cleanable.dirty.ratio` = 0.5
- `delete.retention.ms` = 86400000 (1 day)
- `segment.ms` = 43200000 (12 hours)

Which of the following statements correctly describes the behavior of log compaction and deletion for this topic?

1. Log compaction and deletion are mutually exclusive; only one policy can be active at any time.
2. Log compaction will occur once 50% of the segment data is marked as dirty, and logs older than 1 day will be deleted.
3. Deleted records are removed immediately from the log; `delete.retention.ms` specifies the retention time for all records.
4. `segment.ms` dictates the maximum lifespan of any record in the log, after which it is eligible for compaction or deletion.

Choose the correct answer.

Response:

The correct answer is **2. Log compaction will occur once 50% of the segment data is marked as dirty, and logs older than 1 day will be deleted.**

Explanation:

1. **Incorrect**. The configuration `cleanup.policy` = "compact,delete" allows for both log compaction and deletion policies to be applied to the same topic. This means that the log will undergo compaction to remove duplicates and retain only the latest value for each key, and log segments can also be deleted based on time or size constraints.

2. **Correct**. With `min.cleanable.dirty.ratio` set to 0.5, log compaction is triggered when at least 50% of the segment is considered dirty, which means it contains records that are either duplicates or marked for deletion. The `delete.retention.ms` setting ensures that a record marked for deletion remains in the log for an additional day after being deleted, which allows for consumer recovery in case of deletion mistakes before the log is compacted.

3. **Incorrect**. Deleted records are not removed immediately; instead, they are marked for deletion and actually removed during the next compaction cycle. The `delete.retention.ms` parameter specifies the time a deleted record is retained before being permanently removed from the log as part of compaction, not the retention time for all records.

4. **Incorrect**. `segment.ms` specifies the time after which Kafka will close the current log segment and create a new one. It does not dictate the maximum lifespan of any record in the log. Record lifespan is determined by the `cleanup.policy` and associated configurations like `log.retention.ms` for deletion and `min.cleanable.dirty.ratio` for compaction.

Question 3:
In a Kafka cluster, the Controller is a critical component for managing cluster state. Which of the following statements accurately describe the role and election of the Controller? (Select two)

1. Elected by broker majority.
2. Elected by Zookeeper ensemble.
3. Responsible for partition leader election.
4. Manages consumer group offsets.
5. Automatically assigns replicas to brokers based on load.

Choose the correct answers.

Response:

The correct answers are **2. Elected by Zookeeper ensemble.** and **3. Responsible for partition leader election.**

Explanation:

1. **Incorrect**. The Controller in a Kafka cluster is not elected by a broker majority. The election process is not based on a majority vote among the brokers themselves.

2. **Correct**. The Controller is elected by the Zookeeper ensemble. Kafka uses Zookeeper to manage cluster metadata and to perform leader election for both the controller and partition leaders. When the current Controller fails or loses connection to Zookeeper, a new Controller is elected by Zookeeper from among the live members of the cluster.

3. **Correct**. One of the primary responsibilities of the Controller is to manage partition leader elections. When a partition leader fails, the Controller is responsible for choosing a new leader from the set of in-sync replicas (ISRs) and updating the cluster metadata accordingly.

4. **Incorrect**. Managing consumer group offsets is not a responsibility of the Controller. Consumer group offsets are maintained by Kafka brokers, with the offsets being stored either in a dedicated __consumer_offsets topic within Kafka (for newer versions) or Zookeeper (for older versions).

5. **Incorrect**. Automatically assigning replicas to brokers based on load is not directly managed by the Controller. Replica assignment is typically done at topic creation time or during manual rebalancing operations. While the Controller does manage some aspects of replica management, such as initiating reassignment tasks, the automatic balancing of load is not a direct responsibility of the Controller but can be achieved through tools like Kafka's built-in partition reassignment tool or third-party solutions.

Question 4:
In the context of Kafka's distributed architecture, broker elections are vital for cluster health and stability. Consider the following advanced scenarios where Kafka's internal mechanisms must decide on leadership roles:

1. If a broker acting as the Controller goes down, what mechanism is responsible for the election of a new Controller?
2. A partition leader fails, and all its replicas are on brokers with the same network latency to the Zookeeper ensemble. How is the new leader chosen among the replicas?
3. During a network partition, a subset of brokers becomes isolated from the main cluster. What determines which brokers will continue to serve as leaders for their partitions?

Select the most accurate answers for each scenario:

A. The Zookeeper ensemble elects the new Controller based on ephemeral node creation sequence.
B. The new partition leader is elected based on the ISR list order, favoring replicas with the most recent updates.
C. Brokers in the main cluster segment with access to Zookeeper retain their roles, while isolated brokers step down until connectivity is restored.
D. A broker majority within the isolated segment elects a new temporary Controller until the network partition is resolved.
E. The election of a new partition leader among replicas with identical network latency is determined by a random selection process.

Choose the correct answers.

Response:

1. **A. The Zookeeper ensemble elects the new Controller based on ephemeral node creation sequence.**
   
   Explanation: When the broker acting as the Controller fails, Zookeeper plays a crucial role in the election of a new Controller. Kafka brokers register themselves with Zookeeper using ephemeral nodes. When the current Controller's node disappears from Zookeeper (due to failure or disconnection), Zookeeper triggers the Controller re-election process among the live brokers. The new Controller is typically the first broker to respond to this trigger, based on the sequence of ephemeral node creation.

2. **B. The new partition leader is elected based on the ISR list order, favoring replicas with the most recent updates.**
   
   Explanation: Kafka does not use a random process or explicit network latency measurements to select a new leader among replicas. Instead, it relies on the ordered list of in-sync replicas (ISRs) for each partition. The new leader is usually the first replica in the ISR list that is still available. This mechanism ensures that the chosen leader is up-to-date with the latest messages to prevent data loss.

3. **C. Brokers in the main cluster segment with access to Zookeeper retain their roles, while isolated brokers step down until connectivity is restored.**
   
   Explanation: In the event of a network partition that isolates a subset of brokers, the decision on which brokers continue to serve as leaders for their partitions depends on their ability to communicate with Zookeeper. Brokers on the side of the partition that maintains connectivity to Zookeeper continue to function normally, retaining their roles. Meanwhile, isolated brokers lose their leadership status for partitions and step down, becoming followers if they are part of the ISR and can establish leadership once connectivity is restored and they rejoin the cluster. This ensures the cluster remains operational and consistent, prioritizing segments with Zookeeper connectivity.

D and E are incorrect options based on Kafka's current architecture and leader election protocols.

Question 5:
A Kafka producer is configured to use the `acks=all` setting while publishing messages to a topic partition that has a replication factor of 3. Broker A hosts the current leader for this partition, while Brokers B and C host the replicas. Due to unforeseen circumstances, both Broker B and Broker C go offline simultaneously. What is the impact on the producer's ability to successfully publish messages to this partition?

1. The producer will be able to publish messages, but with potential data loss.
2. The producer will temporarily be unable to publish messages until at least one replica broker comes back online.
3. The producer will continue to publish messages successfully without any impact.
4. The producer will immediately switch to another topic's partition that has all replicas available.

Choose the correct answer.

Response:

The correct answer is **2. The producer will temporarily be unable to publish messages until at least one replica broker comes back online.**

Explanation:

1. **Incorrect**. With `acks=all`, the producer requires acknowledgments from the leader and all in-sync replicas (ISRs) to consider a message write successful. If Brokers B and C are offline, it's not a matter of potential data loss but rather that the producer cannot achieve the required acknowledgments from all replicas, as they are not available to replicate the data.

2. **Correct**. The `acks=all` setting ensures that the producer receives acknowledgments from the partition leader and all replicas before considering a message successfully published. If Brokers B and C, hosting the replicas, go offline, the condition for `acks=all` cannot be met because these replicas cannot acknowledge the message replication. As a result, the producer will be unable to publish new messages to this partition until at least one of the replicas (Broker B or C) becomes available again and can acknowledge the message replication alongside the leader (Broker A). 

If Brokers B and C were considered part of the ISR list before going down, the producer would be unable to publish messages because it expects acknowledgments from all ISRs, which now includes unavailable brokers. This state creates a temporary inability to publish new messages, as the producer cannot satisfy its acknowledgment requirements.

However, the immediate effect of brokers going offline is that they are removed from the ISR for that partition. If the leader (Broker A) remains online but all replicas are offline, the ISR shrinks to include only the leader. In real-time operation, Kafka aims to maintain availability and durability, so the producer can still publish messages, but only if at least one replica comes back online to fulfill the acks=all requirement of replicating to all in-sync replicas. This detail was overlooked in the initial explanation.

3. **Incorrect**. The `acks=all` configuration means that the producer expects acknowledgments from all replicas to ensure data durability. If the replicas are down, the producer won't be able to receive acknowledgments from all required parties, impacting its ability to successfully publish messages. It's not about continuing without impact; the producer will face a temporary blockade in publishing messages.

4. **Incorrect**. Kafka producers do not automatically switch to another topic's partition in response to issues with the current partition's replicas. The producer's target partition is determined by the partitioning logic (either default or custom) at the time of message production. Failures of replicas in a partition do not trigger automatic redirection of messages to different partitions within the same or different topics.

