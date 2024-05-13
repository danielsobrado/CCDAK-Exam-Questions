## Question 1

Which of the following is stored in Zookeeper for a Kafka cluster? (Select two)

A. Consumer offsets
B. Kafka broker information
C. Topic partition assignments
D. Topic-level configurations
E. Producer client IDs

**Answer:** B, D

**Explanation:**
In a Kafka cluster, Zookeeper is used to store critical cluster metadata. This includes:

- Kafka broker information: Details about each broker in the cluster.
- Topic-level configurations: Topic configurations such as retention policies, replication factors, etc.

The other options are stored elsewhere:

- A: Consumer offsets are stored in the `__consumer_offsets` topic in Kafka itself, not in Zookeeper.
- C: Topic partition assignments are managed by the Kafka controller, not stored in Zookeeper.
- E: Producer client IDs are not stored in Zookeeper. They are just identifiers used by the producer clients.
