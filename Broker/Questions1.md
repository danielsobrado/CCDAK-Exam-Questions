### Question 1:
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

### Question 2:
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

### Question 3:
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

### Question 4:
In the context of Kafka's distributed architecture, broker elections are vital for cluster health and stability. Consider the following advanced scenarios where Kafka's internal mechanisms must decide on leadership roles:

1. If a broker acting as the Controller goes down, what mechanism is responsible for the election of a new Controller?
2. A partition leader fails, and all its replicas are on brokers with the same network latency to the Zookeeper ensemble. How is the new leader chosen among the replicas?
3. During a network partition, a subset of brokers becomes isolated from the main cluster. What determines which brokers will continue to serve as leaders for their partitions?

Select the most accurate answers for each scenario:

- A. The Zookeeper ensemble elects the new Controller based on ephemeral node creation sequence.
- B. The new partition leader is elected based on the ISR list order, favoring replicas with the most recent updates.
- C. Brokers in the main cluster segment with access to Zookeeper retain their roles, while isolated brokers step down until connectivity is restored.
- D. A broker majority within the isolated segment elects a new temporary Controller until the network partition is resolved.
- E. The election of a new partition leader among replicas with identical network latency is determined by a random selection process.

Choose the correct answers.

Response:

1. **- A. The Zookeeper ensemble elects the new Controller based on ephemeral node creation sequence.**
   
   Explanation: When the broker acting as the Controller fails, Zookeeper plays a crucial role in the election of a new Controller. Kafka brokers register themselves with Zookeeper using ephemeral nodes. When the current Controller's node disappears from Zookeeper (due to failure or disconnection), Zookeeper triggers the Controller re-election process among the live brokers. The new Controller is typically the first broker to respond to this trigger, based on the sequence of ephemeral node creation.

2. **B. The new partition leader is elected based on the ISR list order, favoring replicas with the most recent updates.**
   
   Explanation: Kafka does not use a random process or explicit network latency measurements to select a new leader among replicas. Instead, it relies on the ordered list of in-sync replicas (ISRs) for each partition. The new leader is usually the first replica in the ISR list that is still available. This mechanism ensures that the chosen leader is up-to-date with the latest messages to prevent data loss.

3. **C. Brokers in the main cluster segment with access to Zookeeper retain their roles, while isolated brokers step down until connectivity is restored.**
   
   Explanation: In the event of a network partition that isolates a subset of brokers, the decision on which brokers continue to serve as leaders for their partitions depends on their ability to communicate with Zookeeper. Brokers on the side of the partition that maintains connectivity to Zookeeper continue to function normally, retaining their roles. Meanwhile, isolated brokers lose their leadership status for partitions and step down, becoming followers if they are part of the ISR and can establish leadership once connectivity is restored and they rejoin the cluster. This ensures the cluster remains operational and consistent, prioritizing segments with Zookeeper connectivity.

D and E are incorrect options based on Kafka's current architecture and leader election protocols.

### Question 5:
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

2. **Correct**. The `acks=all` setting ensures that the producer receives acknowledgments from the partition leader and all replicas before considering a message successfully published. If Brokers B and C, hosting the replicas, go offline, the condition for `acks=all` cannot be met because these replicas cannot acknowledge the message replication. As a result, the producer will be unable to publish new messages to this partition until at least one of the replicas (Broker B or C) becomes available again and can acknowledge the message replication alongside the leader (Broker A). If Brokers B and C were considered part of the ISR list before going down, the producer would be unable to publish messages because it expects acknowledgments from all ISRs, which now includes unavailable brokers. This state creates a temporary inability to publish new messages, as the producer cannot satisfy its acknowledgment requirements. However, the immediate effect of brokers going offline is that they are removed from the ISR for that partition. If the leader (Broker A) remains online but all replicas are offline, the ISR shrinks to include only the leader. In real-time operation, Kafka aims to maintain availability and durability, so the producer can still publish messages, but only if at least one replica comes back online to fulfill the acks=all requirement of replicating to all in-sync replicas. This detail was overlooked in the initial explanation. The immediate effect of the replica brokers going offline is that they are removed from the in-sync replica (ISR) list for the partition. Note that if the leader (Broker A) remains online, the producer can still publish messages to the partition, but the `acks=all` requirement ensures that the messages are replicated to all available ISRs before being considered successfully published. So, the producer's ability to publish depends on the presence of at least one ISR, which in this case would be just the leader until a replica comes back online.

3. **Incorrect**. The `acks=all` configuration means that the producer expects acknowledgments from all replicas to ensure data durability. If the replicas are down, the producer won't be able to receive acknowledgments from all required parties, impacting its ability to successfully publish messages. It's not about continuing without impact; the producer will face a temporary blockade in publishing messages.  Kafka aims to maintain a balance between availability and durability. While the `acks=all` setting prioritizes durability, Kafka's design allows for continued operation with a reduced ISR to maintain availability, as long as the minimum ISR count is met.

4. **Incorrect**. Kafka producers do not automatically switch to another topic's partition in response to issues with the current partition's replicas. The producer's target partition is determined by the partitioning logic (either default or custom) at the time of message production. Failures of replicas in a partition do not trigger automatic redirection of messages to different partitions within the same or different topics.

### Question 6:
When a Kafka broker starts up, it performs various initialization tasks. Which of the following is NOT one of these tasks?

1. Registering itself with Zookeeper.
2. Loading the replica assignment for each partition it hosts.
3. Creating a new Zookeeper znode for each topic it has partitions for.
4. Initializing the log directories for each partition it hosts.

Choose the correct answer.

Response:

The correct answer is **3. Creating a new Zookeeper znode for each topic it has partitions for.**

Explanation:

1. **Incorrect**. When a Kafka broker starts up, one of the first tasks it performs is registering itself with Zookeeper. It creates an ephemeral znode under the `/brokers/ids` path in Zookeeper, which signifies its presence in the cluster.

2. **Incorrect**. The broker loads the replica assignment for each partition it hosts from Zookeeper during startup. This information is stored in Zookeeper under the `/brokers/topics` path and contains the mapping of partitions to their assigned replicas.

3. **Correct**. A Kafka broker does not create new Zookeeper znodes for each topic it has partitions for during startup. The topic znodes are created when the topics themselves are created, either through the Kafka admin tools or via the Kafka broker when it receives a request to create a new topic.

4. **Incorrect**. Initializing the log directories for each partition it hosts is an essential task performed by the broker during startup. It ensures that the necessary directory structure and files are in place for storing and managing the partition data.

### Question 7:
In a Kafka cluster, the Controller is responsible for managing partition states and leadership. How does the Controller ensure that partition leadership is evenly distributed among the brokers in the cluster?

1. The Controller periodically triggers a rebalance operation to redistribute partition leadership.
2. The Controller assigns leadership to the broker with the least number of leader partitions for each new partition.
3. The Controller uses a round-robin algorithm to assign leadership across brokers.
4. The Controller does not actively manage the distribution of partition leadership among brokers.

Choose the correct answer.

Response:

The correct answer is **4. The Controller does not actively manage the distribution of partition leadership among brokers.**

Explanation:

1. **Incorrect**. The Controller does not periodically trigger rebalance operations to redistribute partition leadership. Rebalancing is typically initiated by Kafka administrators or automated tools based on cluster performance and resource utilization.

2. **Incorrect**. The Controller does not assign leadership based on the number of leader partitions each broker currently has. When a partition leader needs to be elected, the Controller selects the first replica in the ISR (in-sync replica) list, regardless of the broker's current leadership count.

3. **Incorrect**. The Controller does not use a round-robin algorithm to assign leadership across brokers. The leader election process is based on the ISR list and the availability of replicas, rather than a predetermined order or rotation.

4. **Correct**. The Controller's primary responsibility is to manage partition states and elect partition leaders when necessary, such as when a broker fails or a new partition is created. However, it does not actively aim to evenly distribute partition leadership among brokers. The distribution of partition leadership is a result of factors like topic creation, replica assignment, and broker failures, rather than being actively managed by the Controller.

### Question 8:
Consider a Kafka cluster with 5 brokers and a topic with 10 partitions and a replication factor of 3. The cluster experiences a network partition, splitting the brokers into two groups: Group A with 2 brokers and Group B with 3 brokers. Both groups can communicate with Zookeeper. How does Kafka handle partition leadership in this scenario?

1. Partitions with a leader in Group A will continue to function normally, while partitions with a leader in Group B will be offline until the network partition is resolved.
2. Partitions with a leader in Group B will continue to function normally, while partitions with a leader in Group A will elect new leaders from the ISRs in Group B.
3. All partitions will be offline until the network partition is resolved, as the brokers cannot reach a quorum for leader election.
4. The behavior is non-deterministic and depends on which group the Controller belongs to.

Choose the correct answer.

Response:

The correct answer is **2. Partitions with a leader in Group B will continue to function normally, while partitions with a leader in Group A will elect new leaders from the ISRs in Group B.**

Explanation:

1. **Incorrect**. In a network partition scenario where both groups can communicate with Zookeeper, Kafka prioritizes availability and allows the larger group (Group B in this case) to continue operating normally. Partitions with a leader in Group A will not continue to function normally, as they do not have a majority of ISRs available.

2. **Correct**. Kafka's partition leadership election process is designed to ensure that the cluster remains available and functional in the presence of network partitions. When a network partition occurs, Kafka follows the "majority rule" to determine which group of brokers should remain active. In this scenario, Group B has a majority of brokers (3 out of 5) and can communicate with Zookeeper. Therefore, partitions with a leader in Group B will continue to function normally. For partitions with a leader in Group A, the brokers in Group B will detect the loss of the leader and elect a new leader from the available ISRs within Group - B. This ensures that all partitions have a functioning leader and can continue to serve client requests.

3. **Incorrect**. Not all partitions will be offline in this scenario. As long as a group of brokers (in this case, Group B) has a majority and can communicate with Zookeeper, Kafka will allow that group to continue operating and serving client requests for the partitions they hold the leadership for.

4. **Incorrect**. The behavior of partition leadership in a network partition scenario is deterministic and does not depend on which group the Controller belongs to. Kafka's partition leadership election process is designed to prioritize availability and maintain consistency based on the majority of brokers and their ability to communicate with Zookeeper, regardless of the Controller's location.

### Question 9:
In a Kafka cluster, a broker is configured with the following settings:
- `num.io.threads=8`
- `num.network.threads=4`
- `num.replica.fetchers=2`

What do these configurations control in terms of the broker's performance and resource utilization?

1. `num.io.threads` controls the number of threads used for disk I/O operations, `num.network.threads` controls the number of threads used for network I/O operations, and `num.replica.fetchers` controls the number of threads used for fetching messages from the leader replica.

2. `num.io.threads` controls the number of threads used for network I/O operations, `num.network.threads` controls the number of threads used for disk I/O operations, and `num.replica.fetchers` controls the number of threads used for fetching messages from the leader replica.

3. `num.io.threads` controls the number of threads used for disk I/O operations, `num.network.threads` controls the number of threads used for network I/O operations, and `num.replica.fetchers` controls the number of threads used for replicating messages to follower replicas.

4. `num.io.threads` controls the number of threads used for replicating messages to follower replicas, `num.network.threads` controls the number of threads used for disk I/O operations, and `num.replica.fetchers` controls the number of threads used for network I/O operations.

Choose the correct answer.

Response:

The correct answer is **1. `num.io.threads` controls the number of threads used for disk I/O operations, `num.network.threads` controls the number of threads used for network I/O operations, and `num.replica.fetchers` controls the number of threads used for fetching messages from the leader replica.**

Explanation:

1. **Correct**. The given configurations control different aspects of the broker's performance and resource utilization:
   - `num.io.threads` specifies the number of I/O threads the broker uses for disk I/O operations, such as reading and writing message data to disk.
   - `num.network.threads` specifies the number of network threads the broker uses for handling network I/O operations, such as accepting client connections and processing requests.
   - `num.replica.fetchers` specifies the number of threads used by the broker to fetch messages from the leader replica for partitions it is a follower for. These threads are responsible for replicating data from the leader to the follower replicas.

2. **Incorrect**. The description of `num.io.threads` and `num.network.threads` is swapped in this option. `num.io.threads` is used for disk I/O operations, while `num.network.threads` is used for network I/O operations.

3. **Incorrect**. The description of `num.replica.fetchers` is incorrect in this option. `num.replica.fetchers` controls the number of threads used for fetching messages from the leader replica, not for replicating messages to follower replicas.

4. **Incorrect**. The descriptions of all three configurations are incorrect in this option. `num.io.threads` is used for disk I/O operations, `num.network.threads` is used for network I/O operations, and `num.replica.fetchers` is used for fetching messages from the leader replica.

### Question 10:
A Kafka broker is configured with the following settings:
- `log.segment.bytes=1073741824`
- `log.segment.ms=86400000`
- `log.retention.bytes=-1`
- `log.retention.ms=604800000`

Based on these configurations, when will Kafka start a new log segment for a partition, and how long will the old log segments be retained?

1. Kafka will start a new log segment when the current segment reaches 1 GB in size or after 24 hours, whichever comes first. Old log segments will be retained for 7 days.

2. Kafka will start a new log segment when the current segment reaches 1 GB in size or after 24 hours, whichever comes first. Old log segments will be retained indefinitely.

3. Kafka will start a new log segment when the current segment reaches 1 GB in size. Old log segments will be retained for 7 days or until the total log size exceeds 1 GB, whichever comes first.

4. Kafka will start a new log segment after 24 hours, regardless of the size. Old log segments will be retained for 7 days or until the total log size exceeds 1 GB, whichever comes first.

Choose the correct answer.

Response:

The correct answer is **1. Kafka will start a new log segment when the current segment reaches 1 GB in size or after 24 hours, whichever comes first. Old log segments will be retained for 7 days.**

Explanation:

1. **Correct**. The given configurations control the log segment creation and retention policies for Kafka:
   - `log.segment.bytes=1073741824` specifies that a new log segment should be started when the current segment reaches 1 GB (1073741824 bytes) in size.
   - `log.segment.ms=86400000` specifies that a new log segment should be started after 24 hours (86400000 milliseconds), even if the current segment has not reached the size limit.
   - `log.retention.bytes=-1` indicates that there is no size-based retention limit for log segments. The value -1 means that log segments will not be deleted based on their total size.
   - `log.retention.ms=604800000` specifies that log segments should be retained for 7 days (604800000 milliseconds) before being eligible for deletion.

   Therefore, Kafka will start a new log segment when either the size limit (1 GB) or the time limit (24 hours) is reached, whichever comes first. Old log segments will be retained for 7 days based on the `log.retention.ms` setting.

2. **Incorrect**. While the log segment creation policy is correctly described, the retention policy is incorrect. Old log segments will not be retained indefinitely, as `log.retention.ms` is set to 7 days (604800000 milliseconds).

3. **Incorrect**. The log segment creation policy is partially correct, but it ignores the time-based limit specified by `log.segment.ms`. Additionally, the retention policy is incorrect, as `log.retention.bytes=-1` means that there is no size-based retention limit.

4. **Incorrect**. The log segment creation policy is incorrect, as it ignores the size-based limit specified by `log.segment.bytes`. The retention policy is also incorrect, as `log.retention.bytes=-1` means that there is no size-based retention limit.
### Question 11:
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

### Question 12:
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

### Question 13:
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

### Question 14:
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

### Question 15:
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

## Question 21

Can Kafka's zero-copy optimization be used in combination with compression?

- A. Yes, zero-copy and compression can be used together seamlessly.
- B. No, zero-copy is incompatible with compression and cannot be used together.
- C. Zero-copy can be used with compression, but it requires additional configuration.
- D. Zero-copy is automatically disabled when compression is enabled.

**Answer:** A

**Explanation:**
Kafka's zero-copy optimization can be used in combination with compression seamlessly. Zero-copy and compression are independent features that can work together to optimize data transfer and storage in Kafka.

Here's how zero-copy and compression can be used together:

1. Producer-side compression:
   - Before sending data to Kafka, the producer application can compress the data using a compression algorithm supported by Kafka, such as Gzip, Snappy, or LZ4.
   - Compression reduces the size of the data, which can help save network bandwidth and storage space.

2. Zero-copy data transfer:
   - When the producer sends the compressed data to Kafka, Kafka uses zero-copy optimization to transfer the compressed data directly from the file system cache to the network buffer.
   - Zero-copy operates on the compressed data without any modifications or decompression.

3. Broker-side storage:
   - Kafka brokers store the compressed data as-is, without decompressing it.
   - Storing compressed data helps optimize storage utilization and reduces the storage footprint of the Kafka cluster.

4. Consumer-side decompression:
   - When the consumer receives the compressed data from Kafka, it needs to decompress the data before processing it.
   - The consumer is responsible for decompressing the data using the same compression algorithm used by the producer.

Zero-copy and compression can work together seamlessly because zero-copy operates on the compressed data without any modifications. It transfers the compressed data efficiently from the producer to the consumer, while compression helps reduce the data size and optimize storage.

Using zero-copy with compression does not require any additional configuration (option C) and is not automatically disabled when compression is enabled (option D). Kafka supports the combination of zero-copy and compression out of the box.

By leveraging both zero-copy and compression, Kafka can achieve efficient data transfer, reduced network bandwidth usage, and optimized storage utilization, leading to improved overall performance and scalability of the Kafka cluster.
