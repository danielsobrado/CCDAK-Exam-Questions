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

## Question 4

In the Confluent Schema Registry, what is the default compatibility setting for new schemas?

A. BACKWARD
B. FORWARD
C. FULL
D. NONE

**Answer:** C

**Explanation:**
In the Confluent Schema Registry, when a new schema is registered for a subject, the default compatibility setting is FULL. This means that a new schema must be both backward and forward compatible with the existing schema(s) for that subject.

- BACKWARD compatibility means that data written with a new schema can be read by code using an old schema.
- FORWARD compatibility means that data written with an old schema can be read by code using a new schema.
- FULL compatibility means that both BACKWARD and FORWARD compatibilities are required.
- NONE means that no compatibility checking is performed.

So by default, the Schema Registry enforces the strictest level of compatibility for new schemas. This can be changed on a per-subject basis or globally.

## Question 5

Where does the Confluent Schema Registry store its own configuration?

A. In Zookeeper
B. In a Kafka topic
C. On the filesystem
D. In a database

**Answer:** A

**Explanation:**
The Confluent Schema Registry uses Zookeeper to store its own configuration. When the Schema Registry starts up, it reads its configuration from a Zookeeper path, which defaults to `/schema-registry`.

Some of the key configuration properties stored in Zookeeper include:

- `kafkastore.topic`: The Kafka topic that the Schema Registry uses to store schema data.
- `master.eligibility`: Whether the instance is eligible to be the master.
- `host.name`: The host name to use for the Schema Registry instance.
- `port`: The port to run the Schema Registry instance on.

So while the Schema Registry uses a Kafka topic to store the actual schema data, it uses Zookeeper for its own configuration.

- B is incorrect because while the Schema Registry does use a Kafka topic, it's for schema data, not its own configuration.
- C and D are incorrect because the Schema Registry does not use the filesystem or a database for its configuration.

## Question 6

How does the Confluent Schema Registry ensure high availability?

A. By running multiple instances and electing a master
B. By relying on the availability of the underlying Kafka cluster
C. By replicating data across multiple Zookeeper instances
D. By using a distributed consensus protocol among instances

**Answer:** A

**Explanation:**
The Confluent Schema Registry is designed to be highly available by allowing multiple instances to run in a cluster mode. When running in cluster mode:

- Each instance of the Schema Registry is equal - there is no designated leader or follower.
- Instances communicate with each other via Kafka.
- One instance is elected the "master" at any given time. The master is responsible for handling all write requests (new schemas, config changes, etc.).
- All instances can serve read requests, whether they are the master or not.
- If the master goes down, a new master is automatically elected from the remaining instances.

This master election process ensures that there is always one instance responsible for consistency of writes, while reads can be served from any instance for high availability and scalability.

- B is incorrect because while the Schema Registry does rely on Kafka, it has its own high availability mechanism beyond just relying on Kafka's availability.
- C is incorrect because while the Schema Registry does use Zookeeper, it doesn't replicate its own data across Zookeeper instances for high availability.
- D is incorrect because the Schema Registry doesn't use a distributed consensus protocol like Raft or Paxos among its instances. It uses a simpler master election process.
