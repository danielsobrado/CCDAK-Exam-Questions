## Question 1

Where are the ACLs stored in a Kafka cluster by default?

- A. In the Kafka topic `_acls`
- B. Inside the broker's data directory  
- C. In Zookeeper node `/kafka-acl/`
- D. In a separate ACL server

**Answer:** C

**Explanation:**
ACLs are stored in Zookeeper by default under the `/kafka-acl/` znode. This allows all brokers to access the ACL information in a consistent manner.

- A is incorrect because there is no `_acls` topic used for storing ACLs.
- B is incorrect as ACLs are not stored on the broker's data directories.
- D is incorrect because there is no separate ACL server. ACLs are managed within the Kafka cluster itself.

Note: 
For newer versions of Kafka that operate without Zookeeper, the ACLs are stored in an internal Kafka topic instead of Zookeeper. This change is part of the KIP-500 proposal, which aims to remove Zookeeper from Kafka and improve scalability and management. The internal topic used for storing ACLs in Kafka versions without Zookeeper is named `__cluster_metadata`.

Therefore, for Kafka clusters that do not use Zookeeper, the correct answer would be: The Kafka topic `__cluster_metadata`.

## Question 2

What Kafka CLI command can be used to add new ACL rules to a running Kafka cluster?

- A. `kafka-acls.sh`
- B. `kafka-configs.sh`
- C. `kafka-topics.sh`
- D. `kafka-console-producer.sh`

**Answer:** A

**Explanation:**
The `kafka-acls.sh` CLI tool is used to manage ACLs in Kafka. It allows adding, removing or listing ACL rules in a running cluster.

- B is used for altering configs, not ACLs.
- C is used for managing topics, not ACLs.
- D is used for producing messages, not managing ACLs.

## Question 3

Which of the following is NOT a valid resource type when defining ACLs in Kafka?

- A. Topic
- B. Consumer Group
- C. Cluster
- D. Partition

**Answer:** D

**Explanation:**
In Kafka, ACLs can be defined for resource types like Topic, Consumer Group, Cluster, and others. However, Partition is not a valid resource type for defining ACLs.

- A, B, C are all valid resource types for ACLs.
- D is invalid because ACLs are defined at the topic level, not individual partition level. The topic resource type covers all its partitions.

## Question 4

In the Confluent Schema Registry, what is the default compatibility setting for new schemas?

- A. BACKWARD
- B. FORWARD
- C. FULL
- D. NONE

**Answer:** A

**Explanation:**
The Confluent Schema Registry default compatibility type is BACKWARD . The main reason that BACKWARD compatibility mode is the default, and preferred for Kafka, is so that you can rewind consumers to the beginning of the topic.

- BACKWARD compatibility means that data written with a new schema can be read by code using an old schema.
- FORWARD compatibility means that data written with an old schema can be read by code using a new schema.
- FULL compatibility means that both BACKWARD and FORWARD compatibilities are required.
- NONE means that no compatibility checking is performed.

## Question 5

Where does the Confluent Schema Registry store its own configuration?

- A. In Zookeeper
- B. In a Kafka topic
- C. On the filesystem
- D. In a database

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

- A. By running multiple instances and electing a master
- B. By relying on the availability of the underlying Kafka cluster
- C. By replicating data across multiple Zookeeper instances
- D. By using a distributed consensus protocol among instances

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

## Question 7

What is the purpose of ACLs (Access Control Lists) in Kafka?

- A. To encrypt messages for secure communication between clients and brokers
- B. To authenticate clients and authorize their access to Kafka resources
- C. To compress messages for efficient storage and transmission
- D. To validate the schema of messages produced to Kafka topics

**Explanation:**
ACLs (Access Control Lists) in Kafka are used to authenticate clients and authorize their access to Kafka resources. They provide a mechanism to control and restrict the actions that clients can perform on Kafka brokers, topics, and other resources. ACLs allow you to define permissions for specific users or groups, specifying which operations they are allowed to perform on particular resources. By configuring ACLs, you can enforce security policies and ensure that clients have the appropriate privileges to access and interact with Kafka. ACLs help in securing Kafka clusters by preventing unauthorized access and protecting sensitive data. They are a key component of Kafka's security model, along with other features like authentication and encryption.

**Answer:** B

## Question 8

How are ACLs stored and managed in Kafka?

- A. ACLs are stored in Zookeeper and managed through the Kafka broker configuration
- B. ACLs are stored in a dedicated Kafka topic and managed using Kafka command-line tools
- C. ACLs are stored in a separate ACL server and managed through a REST API
- D. ACLs are stored in the Kafka broker's local file system and managed using configuration files

**Explanation:**
In Kafka, ACLs are stored in Zookeeper and managed through the Kafka broker configuration. When ACLs are configured, they are stored in a dedicated znode in Zookeeper, typically under the `/kafka-acl` path. The Kafka brokers read the ACL information from Zookeeper and enforce the access control rules on incoming client requests. Any changes to ACLs, such as adding or removing permissions, are made through the Kafka broker configuration and are propagated to Zookeeper. Kafka provides command-line tools, such as `kafka-acls.sh`, to manage ACLs by interacting with Zookeeper. These tools allow you to create, list, and delete ACLs for specific resources and principals.

**Answer:** A

## Question 9

What happens when a client tries to perform an operation that is not allowed by the configured ACLs?

- A. The operation is performed, but a warning is logged in the Kafka broker logs
- B. The operation is rejected, and the client receives an authorization error
- C. The operation is performed, but the message is flagged as unauthorized
- D. The operation is delayed until the necessary ACLs are added

**Explanation:**
When a client tries to perform an operation that is not allowed by the configured ACLs, the operation is rejected, and the client receives an authorization error. Kafka brokers enforce the ACLs by checking the permissions of the client against the requested operation and resource. If the client does not have the necessary privileges, the broker denies the operation and returns an authorization error to the client. The client can then handle the error accordingly, such as logging the failure, retrying with different credentials, or propagating the error to the application. The unauthorized operation is not performed, and no data is processed or modified. This behavior ensures that Kafka maintains the integrity and security of the system by strictly enforcing the defined access control rules.

**Answer:** B

## Question 10

What is the purpose of the `CreateTopics` ACL operation in Kafka?

- A. To allow a client to create new topics in a Kafka cluster
- B. To permit a client to produce messages to a specific topic
- C. To grant a client permission to delete topics from a Kafka cluster
- D. To enable a client to modify the configuration of existing topics

**Explanation:**
The `CreateTopics` ACL operation in Kafka is used to allow a client to create new topics in a Kafka cluster. When a client has been granted the `CreateTopics` permission, it is authorized to send requests to the Kafka brokers to create new topics. This ACL operation is typically assigned to administrative clients or applications responsible for managing the topic lifecycle in a Kafka cluster. By default, Kafka brokers are configured to require `CreateTopics` permission for any client attempting to create a new topic. This ensures that only authorized clients can create topics and helps maintain control over the topic management process in the cluster.

**Answer:** A
