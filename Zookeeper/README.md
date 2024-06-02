## ZooKeeper Overview

Apache ZooKeeper used to play a critical role in Kafka's ecosystem, acting as a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services. In newer versions of Kafka Zookeeper is not required anymore.

### Key Concepts

- **Cluster Coordination**: ZooKeeper is used by Kafka for leader election, cluster membership, and topic configuration management.
- **Configuration Management**: Stores configuration data and metadata for Kafka brokers, topics, and other entities.

Zookeeper's internal data structure is a tree:
- Each node is called a zNode
- Each zNode has a path
- zNodes can be persistent or ephemeral
- Each zNode can store data
- You cannot rename a zNode
- Each zNode can be watched for changes

### ZooKeeper Configuration Parameters

Understanding key ZooKeeper configuration parameters is essential for setting up and managing a robust Kafka environment:

- **`data.dir`**: 
  - **Description**: Location of the directory where ZooKeeper stores its data.
  - **Impact**: Affects how ZooKeeper stores and retrieves cluster state and metadata.

- **`maxClientCnxns`**:
  - **Description**: Maximum number of client connections that can be concurrently handled by a ZooKeeper server. Zero means unlimited.
  - **Impact**: Balances load handling capacity with resource utilization.

- **`tickTime`**:
  - **Description**: The length of a single tick in milliseconds, which is the basic time unit in ZooKeeper's time management. Commonly set to 2000.
  - **Impact**: Influences session timeouts and heartbeat intervals for cluster management.

- **`initLimit`**:
  - **Description**: Specifies the amount of time, in ticks (as defined by `tickTime`), to allow followers to connect and sync to a leader. Commonly set to 10.
  - **Impact**: Affects startup and recovery times for ZooKeeper quorums.

- **`syncLimit`**:
  - **Description**: Defines how many ticks can pass between sending a request and receiving an acknowledgment. Often set to 5.
  - **Impact**: Impacts the tolerance for network latency between ZooKeeper nodes.

- **`autopurge.snapRetainCount`**:
  - **Description**: The number of most recent snapshots and the corresponding transaction logs in the dataDir and dataLogDir to retain. Additional files are deleted during purge. Often set to 3.
  - **Impact**: Controls disk space usage by managing the retention of snapshot files.

- **`autopurge.purgeInterval`**:
  - **Description**: The interval, in hours, at which the purge task is triggered. Set to 24 hours to purge once a day.
  - **Impact**: Automates the cleanup process to manage disk space effectively.

### Important ZooKeeper Operations

- **zkCli.sh**: The ZooKeeper command-line interface to interact with the ZooKeeper ensemble.
  ```
  # List the brokers registered in ZooKeeper
  zkCli.sh -server localhost:2181 ls /brokers/ids
  ```
- **Configuration**: Understanding the ZooKeeper configuration file (`zoo.cfg`) is essential for setting up and managing a ZooKeeper ensemble.
- **Monitoring and Maintenance**: Familiarity with ZooKeeper's metrics and logs for monitoring the health and performance of the ensemble.

### ZooKeeper Ensemble

- **Definition**: A ZooKeeper ensemble is a cluster of ZooKeeper servers deployed across multiple nodes to ensure fault tolerance and high availability.
- **Quorum Calculation**: The ensemble follows a 2n + 1 formula, where n is the number of allowed failed servers. This odd number configuration enables majority-based leader elections and operational quorum maintenance.
- **Example**: To tolerate the failure of 2 servers, an ensemble requires 5 servers (2*2+1 = 5).

### High Availability and Fault Tolerance

- The ensemble allows for up to `n` failed servers while still maintaining the quorum. Losing the quorum (more than `n` failures in a 2n + 1 configuration) renders the ZooKeeper cluster non-operational.
- **Quorum Loss Impact**: If quorum is lost, the ZooKeeper service will cease to function, affecting the dependent Kafka cluster's metadata management and overall operation.

### Multi-Node Configuration Parameters

- **`initLimit` and `syncLimit`**: These parameters control the time allowed for server synchronization with the leader during startup (`initLimit`) and operation (`syncLimit`).
    - Given `tickTime=2000`, `initLimit=5`, and `syncLimit=2`, a follower server has up to 10000ms (5 * 2000ms) to initialize with the leader and can be out of sync for up to 4000ms (2 * 2000ms) without being considered down.
  
### Ensemble Membership Configuration

- **`server.<myid>`**: Defines the ensemble membership and communication ports.
    - **Format**: `server.<myid>=<hostname>:<leaderport>:<electionport>`
    - **`myid`**: A unique server identification number within the ensemble. It's specified in a `myid` file located in the ZooKeeper's `dataDir`. This ID correlates with one of the ensemble configurations.
    - **`leaderport`**: Used by following servers to connect to the current leader.
    - **`electionport`**: Facilitates leader election among ensemble members.

### Four Letter Words (4LW) Commands

ZooKeeper supports various "Four Letter Words" commands for monitoring and administration purposes. To enable these commands, the following property must be set in `zoo.cfg`:

- **`4lw.commands.whitelist=*`**: This configuration whitelists all available 4LW commands for use.

#### Key 4LW Commands

- **`ruok`**: Checks if ZooKeeper server is running without errors.
  ```
  echo "ruok" | nc localhost 2181; echo
  ```
- **`conf`**: Prints the server's current configuration.
- **`cons`**: Lists details about client connections to the server.
- **`crst`**: Resets connection and session statistics.
- **`envi`**: Displays the server's environment variables.
- **`srst`**: Resets server statistics.
- **`mntr`**: Outputs a list of variables that can be used for monitoring the health of the cluster.
- **`dump`**: Lists all sessions and ephemeral nodes. This command only works on the leader node.

### Znodes in ZooKeeper

ZooKeeper manages data in a hierarchical namespace, similar to a filesystem, which consists of znodes. There are two main types of znodes:

#### Persistent Znodes

- **Characteristics**: Permanent and must be explicitly deleted by the client. They remain available even after the session that created them ends.
- **Use Cases**: Useful for storing configuration information or metadata that must persist beyond the lifespan of the creating session.

#### Ephemeral Znodes

- **Characteristics**: Temporary and automatically deleted when the session that created them ends. These znodes are crucial for detecting client disconnections.
- **Use Cases**: Often used for maintaining cluster membership and leader election, as they provide a straightforward way to detect when a node has failed or left the cluster.

### Best Practices

- **Security**: Secure ZooKeeper ensemble using ACLs and by enabling SSL for client-server communications.
- **Backup and Recovery**: Regularly backup ZooKeeper data and understand the procedures for recovery in case of data loss.

### Troubleshooting

- **Connectivity Issues**: Solving common connectivity problems between Kafka and ZooKeeper, such as ensuring the correct port and host are specified in Kafka's `zookeeper.connect` configuration.
- **Performance Tuning**: Adjusting ZooKeeper settings for optimal performance, especially in larger Kafka deployments.

### Useful Commands

Here are some useful ZooKeeper commands for managing Kafka's metadata:

- Checking the health of ZooKeeper:
  ```
  echo ruok | nc localhost 2181
  ```
  If ZooKeeper is running fine, it responds with `imok`.

- Listing topics (via ZooKeeper):
  ```
  zkCli.sh -server localhost:2181 ls /brokers/topics
  ```

## KRaft Overview

- KRaft (pronounced craft) mode is generally available starting with Confluent Platform version 7.4
- KRaft is the consensus protocol introduced to remove Kafka's dependency on ZooKeeper for metadata management
- Simplifies Kafka's architecture by consolidating responsibility for metadata into Kafka itself

### Why move to KRaft?
- Enables right-sized clusters to scale up to millions of partitions
- **Near-instantaneous metadata failover**
- Single management model for authentication, configuration, protocols, and networking

### The Controller Quorum
- KRaft controller nodes comprise a Raft quorum which manages the Kafka metadata log
- The leader of the metadata log is called the **active controller**
- Follower controllers replicate data and serve as **hot standbys**
- Requires a **majority** of nodes to be running (e.g., 3-node cluster can survive 1 failure)
- Controllers periodically write out a **snapshot** of the metadata to disk

### Scaling Kafka with KRaft
- KRaft mode is designed to handle a **much larger number of partitions** per cluster compared to ZooKeeper
- Experiment shows KRaft can handle **10 times** the maximum number of partitions for a cluster running ZooKeeper
- Controlled **shutdown time and recovery time** after uncontrolled shutdown are greatly improved with a quorum controller versus ZooKeeper

### Configure Confluent Platform with KRaft
- Client configurations are not impacted by Confluent Platform moving to KRaft

### Limitations and Known Issues
- Combined mode (Kafka node acting as a broker and KRaft controller) is not currently supported for production workloads
- JBOD (just a bunch of disks) is not supported in KRaft mode
- Source-initiated Cluster Linking for Confluent Platform between a source cluster running Confluent Platform 7.0.x or earlier and a destination Confluent Platform cluster running in KRaft mode is not supported
- No support for quorum reconfiguration (adding or removing KRaft controllers)
- Cannot currently use Schema Registry Topic ACL Authorizer for Confluent Platform for Schema Registry with Confluent Platform in KRaft mode
- Health+ reports KRaft controllers as brokers, which may cause alerts to function unexpectedly

See the Kraft section on the [Last minute review](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/LAST_MINUTE_REVIEW.md)