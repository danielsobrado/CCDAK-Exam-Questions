### Question 1

Where are the ACLs stored in a Kafka cluster by default?

A. In the Kafka topic `_acls`
B. Inside the broker's data directory  
C. In Zookeeper node `/kafka-acl/`
D. In a separate ACL server

**Answer:** C

**Explanation:**
ACLs are stored in Zookeeper by default under the `/kafka-acl/` znode. This allows all brokers to access the ACL information in a consistent manner.

- A is incorrect because there is no `_acls` topic used for storing ACLs.
- B is incorrect as ACLs are not stored on the broker's data directories.
- D is incorrect because there is no separate ACL server. ACLs are managed within the Kafka cluster itself.

Note: 
For newer versions of Kafka that operate without Zookeeper, the ACLs are stored in an internal Kafka topic instead of Zookeeper. This change is part of the KIP-500 proposal, which aims to remove Zookeeper from Kafka and improve scalability and management. The internal topic used for storing ACLs in Kafka versions without Zookeeper is named `__cluster_metadata`.

Therefore, for Kafka clusters that do not use Zookeeper, the correct answer would be: The Kafka topic `__cluster_metadata`.

### Question 2

What Kafka CLI command can be used to add new ACL rules to a running Kafka cluster?

A. `kafka-acls.sh`
B. `kafka-configs.sh`
C. `kafka-topics.sh`
D. `kafka-console-producer.sh`

**Answer:** A

**Explanation:**
The `kafka-acls.sh` CLI tool is used to manage ACLs in Kafka. It allows adding, removing or listing ACL rules in a running cluster.

- B is used for altering configs, not ACLs.
- C is used for managing topics, not ACLs.
- D is used for producing messages, not managing ACLs.

### Question 3

Which of the following is NOT a valid resource type when defining ACLs in Kafka?

A. Topic
B. Consumer Group
C. Cluster
D. Partition

**Answer:** D

**Explanation:**
In Kafka, ACLs can be defined for resource types like Topic, Consumer Group, Cluster, and others. However, Partition is not a valid resource type for defining ACLs.

- A, B, C are all valid resource types for ACLs.
- D is invalid because ACLs are defined at the topic level, not individual partition level. The topic resource type covers all its partitions.
