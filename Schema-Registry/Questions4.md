## Question 31

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

## Question 32

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
