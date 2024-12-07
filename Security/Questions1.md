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

What is the purpose of ACLs (Access Control Lists) in Kafka?

- A. To encrypt messages for secure communication between clients and brokers
- B. To authenticate clients and authorize their access to Kafka resources
- C. To compress messages for efficient storage and transmission
- D. To validate the schema of messages produced to Kafka topics

**Explanation:**
ACLs (Access Control Lists) in Kafka are used to authenticate clients and authorize their access to Kafka resources. They provide a mechanism to control and restrict the actions that clients can perform on Kafka brokers, topics, and other resources. ACLs allow you to define permissions for specific users or groups, specifying which operations they are allowed to perform on particular resources. By configuring ACLs, you can enforce security policies and ensure that clients have the appropriate privileges to access and interact with Kafka. ACLs help in securing Kafka clusters by preventing unauthorized access and protecting sensitive data. They are a key component of Kafka's security model, along with other features like authentication and encryption.

**Answer:** B

## Question 5

How are ACLs stored and managed in Kafka?

- A. ACLs are stored in Zookeeper and managed through the Kafka broker configuration
- B. ACLs are stored in a dedicated Kafka topic and managed using Kafka command-line tools
- C. ACLs are stored in a separate ACL server and managed through a REST API
- D. ACLs are stored in the Kafka broker's local file system and managed using configuration files

**Explanation:**
In Kafka, ACLs are stored in Zookeeper and managed through the Kafka broker configuration. When ACLs are configured, they are stored in a dedicated znode in Zookeeper, typically under the `/kafka-acl` path. The Kafka brokers read the ACL information from Zookeeper and enforce the access control rules on incoming client requests. Any changes to ACLs, such as adding or removing permissions, are made through the Kafka broker configuration and are propagated to Zookeeper. Kafka provides command-line tools, such as `kafka-acls.sh`, to manage ACLs by interacting with Zookeeper. These tools allow you to create, list, and delete ACLs for specific resources and principals.

**Answer:** A

## Question 6

What happens when a client tries to perform an operation that is not allowed by the configured ACLs?

- A. The operation is performed, but a warning is logged in the Kafka broker logs
- B. The operation is rejected, and the client receives an authorization error
- C. The operation is performed, but the message is flagged as unauthorized
- D. The operation is delayed until the necessary ACLs are added

**Explanation:**
When a client tries to perform an operation that is not allowed by the configured ACLs, the operation is rejected, and the client receives an authorization error. Kafka brokers enforce the ACLs by checking the permissions of the client against the requested operation and resource. If the client does not have the necessary privileges, the broker denies the operation and returns an authorization error to the client. The client can then handle the error accordingly, such as logging the failure, retrying with different credentials, or propagating the error to the application. The unauthorized operation is not performed, and no data is processed or modified. This behavior ensures that Kafka maintains the integrity and security of the system by strictly enforcing the defined access control rules.

**Answer:** B

## Question 7

What is the purpose of the `CreateTopics` ACL operation in Kafka?

- A. To allow a client to create new topics in a Kafka cluster
- B. To permit a client to produce messages to a specific topic
- C. To grant a client permission to delete topics from a Kafka cluster
- D. To enable a client to modify the configuration of existing topics

**Explanation:**
The `CreateTopics` ACL operation in Kafka is used to allow a client to create new topics in a Kafka cluster. When a client has been granted the `CreateTopics` permission, it is authorized to send requests to the Kafka brokers to create new topics. This ACL operation is typically assigned to administrative clients or applications responsible for managing the topic lifecycle in a Kafka cluster. By default, Kafka brokers are configured to require `CreateTopics` permission for any client attempting to create a new topic. This ensures that only authorized clients can create topics and helps maintain control over the topic management process in the cluster.

**Answer:** A

## Question 8

What is the difference between `Read` and `Write` ACL operations in Kafka?

- A. `Read` allows consuming messages, while `Write` allows producing messages
- B. `Read` allows producing messages, while `Write` allows consuming messages
- C. `Read` allows modifying topic configurations, while `Write` allows deleting topics
- D. `Read` and `Write` are equivalent and can be used interchangeably

**Explanation:**
In Kafka, the `Read` ACL operation allows a client to consume messages from a specific topic, while the `Write` ACL operation allows a client to produce messages to a specific topic. The `Read` permission grants the client the ability to read and fetch messages from the topic, including the metadata required for consumption. On the other hand, the `Write` permission authorizes the client to send messages to the topic and update its content. It's important to note that `Read` and `Write` operations are distinct and serve different purposes. A client with `Read` permission cannot produce messages, and a client with `Write` permission cannot consume messages. The permissions are specific to the respective operations and should be granted based on the client's intended actions.

**Answer:** A

## Question 9

How can you grant a client permission to describe topics and consumer groups in a Kafka cluster?

- A. Assign the `DescribeConfigs` ACL operation to the client
- B. Grant the `Describe` ACL operation to the client
- C. Provide the `AlterConfigs` ACL operation to the client
- D. Give the `IdempotentWrite` ACL operation to the client

**Explanation:**
To grant a client permission to describe topics and consumer groups in a Kafka cluster, you need to assign the `Describe` ACL operation to the client. The `Describe` permission allows a client to view the metadata and details of topics and consumer groups without the ability to modify or delete them. With the `Describe` ACL, a client can send requests to the Kafka brokers to retrieve information such as the list of partitions, replica assignments, and configuration settings for topics. It can also query the state and membership of consumer groups. The `Describe` ACL is commonly used by monitoring and administrative tools to gather information about the Kafka cluster's state without making any changes to the topics or consumer groups.

**Answer:** B

## Question 10

What is the purpose of the `ssl.keystore.location` and `ssl.keystore.password` configurations in Kafka?

- A. To specify the location and password of the truststore for verifying client certificates
- B. To specify the location and password of the keystore for broker authentication
- C. To specify the location and password of the keystore for client authentication
- D. To specify the location and password of the truststore for broker authentication

**Explanation:**
In Kafka, the `ssl.keystore.location` and `ssl.keystore.password` configurations are used to specify the location and password of the keystore for broker authentication. When SSL/TLS is enabled for inter-broker communication or client-broker communication, each Kafka broker needs to have a keystore that contains its private key and certificate. The `ssl.keystore.location` configuration points to the file path of the keystore on the broker's file system, while the `ssl.keystore.password` configuration provides the password required to access the keystore. These configurations are essential for setting up SSL/TLS authentication on the broker side, allowing the broker to securely authenticate itself to clients and other brokers.

**Answer:** B
