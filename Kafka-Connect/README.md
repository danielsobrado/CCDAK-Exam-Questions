## Kafka Connect Overview

Kafka Connect is a tool specifically designed for streaming data between Apache Kafka and other systems.

- A common framework for Kafka connectors.
- Distributed and standalone modes.
- REST interface.
- Automatic offset management.
- Distributed and scalable by default.
- Streaming/batch integration.

### Key Concepts

- **Connectors**: The components in Kafka Connect that implement the connection to other systems. There are two types of connectors: Source connectors for importing data into Kafka, and Sink connectors for exporting data from Kafka.
- **Tasks**: The actual workers that perform the data import or export. Each connector could be split into multiple tasks that can be distributed over a cluster for scalability and fault tolerance.
- **Converters**: Responsible for converting data between Kafka Connect's internal data format and the format required by external systems.

### Core Components

- **Worker**: The runtime environment where connectors and tasks are executed. Workers can be standalone (single process) or distributed across multiple nodes.
- **REST API**: Kafka Connect provides a REST API for managing connectors and tasks, making it easy to deploy and monitor without writing any code.

### Key Connector configurations

Connector configurations are simple key-value mappings. In standalone mode, these are defined in a properties file and passed to the Connect process on the command line. In distributed mode, they are included in the JSON payload for the request that creates or modifies the connector.

Most configurations are connector-dependent, so they can't be outlined here. However, there are a few common options:

- **name:** Unique name for the connector. Attempting to register again with the same name will fail.
- **connector.class:** The Java class for the connector.
- **tasks.max:** The maximum number of tasks that should be created for this connector. The connector may create fewer tasks if it cannot achieve this level of parallelism.
- **key.converter:** (Optional) Override the default key converter set by the worker.
- **value.converter:** (Optional) Override the default value converter set by the worker.

### Important Operations

- **Configuring Connectors**: Understand how to configure source and sink connectors, including specifying the name of the Kafka topics to use, the key and value converters, and any connector-specific settings.
    ```json
    {
      "name": "example-connector",
      "config": {
        "connector.class": "org.example.MyConnector",
        "tasks.max": "1",
        "topics": "my-topic",
        "key.converter": "org.apache.kafka.connect.storage.StringConverter",
        "value.converter": "org.apache.kafka.connect.json.JsonConverter",
        "value.converter.schemas.enable": "false",
        ...
      }
    }
    ```
- **Deploying and Scaling**: Knowledge on deploying connectors in a distributed mode to leverage Kafka Connect's scalability and fault tolerance.
- **Monitoring and Managing Connectors**: Using the REST API to monitor the health and performance of connectors and tasks, and to dynamically adjust their configuration.

### Best Practices

- **Error Handling and Retry Policies**: Implement robust error handling and retry mechanisms to ensure resilience and reliability.
- **Offset Management**: Proper management of offsets to ensure accurate and reliable data processing.
- **Securing Kafka Connect**: Apply security configurations, including encryption and authentication, to protect data in transit and at rest.

### REST API Operations

Kafka Connect's REST API provides comprehensive control over connectors and tasks, allowing for dynamic management without direct code intervention.

- **Worker Information**: Retrieve details about Kafka Connect workers.
- **Connectors Management**: List, create, update, pause, resume, and delete connectors.
- **Tasks Management**: Monitor and manage the individual tasks of each connector.
- **Configuration Management**: View and update connector configurations.

### Internal Topics of Kafka Connect

Kafka Connect utilizes several internal topics for maintaining its state, configuration, and progress:

- **Connector Config**: Stores connector configurations.
- **Task Config**: Contains task-specific configurations.
- **Offset**: Tracks the progress of source connectors to ensure data consistency.
- **Status**: Records the state and status of connectors and tasks.

These internal topics enable Kafka Connect to recover from failures, ensuring robust and reliable data streaming.

### Configuration Options

#### Common Worker Configurations

- **`bootstrap.servers`**: The Kafka cluster to connect to.
- **`key.converter`** and **`value.converter`**: Converters for record keys and values.
- **`offset.flush.interval.ms`**: Frequency at which to save offsets.
- **`plugin.path`**: Path to directory containing Kafka Connect plugins.

#### Distributed Worker Configurations

- **`group.id`**: Unique identifier for the Kafka Connect cluster.
- **`config.storage.topic`** and **`offset.storage.topic`**: Kafka topics for storing connector and task configurations and offsets.

In distributed mode for Kafka Connect, key features include automatic work balancing, dynamic scaling, and fault tolerance. While configuration parameters are similar to standalone mode, the storage locations and classes differ. In distributed mode, Kafka Connect stores offsets, configurations, and task statuses in Kafka topics. It is recommended to manually create these topics to achieve the desired number of partitions and replication factors.

Execution is similar to standalone mode:

```
bin/connect-distributed.sh config/connect-distributed.properties
```

#### JDBC Sink Configuration Options

- **`connection.url`**, **`connection.user`**, **`connection.password`**: Database connection details.
- **`insert.mode`**: Mode of inserting data into the database.
- **`auto.create`** and **`auto.evolve`**: Automatically create and modify database schemas.

### Connector Types

- **Source Connectors**: Import data from external systems into Kafka.
- **Sink Connectors**: Export data from Kafka to external systems.

### Source Connectors
These connectors ingest data from external systems into Kafka topics.
- **JDBC Source Connector**: Ingests data from relational databases into Kafka.
- **File Source Connector**: Reads files from a file system and ingests their contents into Kafka topics.
- **MQTT Source Connector**: Integrates IoT device data communicated over MQTT protocol into Kafka.
- **Twitter Source Connector**: Streams live tweets directly into Kafka topics.

### Sink Connectors
These connectors export data from Kafka topics to external systems.
- **JDBC Sink Connector**: Exports data from Kafka topics to relational databases.
- **HDFS Sink Connector**: Moves data from Kafka topics into HDFS (Hadoop Distributed File System), commonly used in big data environments.
- **Elasticsearch Sink Connector**: Exports data from Kafka topics into Elasticsearch for search and analytics.
- **S3 Sink Connector**: Stores data from Kafka topics into Amazon S3 for durable, long-term storage.

### MQTT Proxy and Connector
- **MQTT (Message Queuing Telemetry Transport)** is a lightweight messaging protocol for small sensors and mobile devices optimized for high-latency or unreliable networks.
- An **MQTT proxy** allows Kafka to directly ingest data from IoT devices using the MQTT protocol.
- An **MQTT connector** (typically a Kafka Connect connector) serves a similar purpose, facilitating data flow from MQTT-enabled devices into Kafka topics.

### JDBC Connectors
- **JDBC (Java Database Connectivity)** connectors are used within Kafka Connect for integrating Kafka with relational databases.
  - A **JDBC source connector** pulls data from a database into Kafka topics.
  - A **JDBC sink connector** moves data from Kafka topics into a database.

### Single Message Transforms (SMT)
- **SMTs** are Kafka Connect transformations that are applied to data as it passes through a connector, allowing for modification of the data (such as enrichment, filtering, or transformation) before it lands in the destination.

### Data Enrichment
- **Data enrichment** in the context of Kafka involves augmenting messages in a stream with additional information. This can be achieved by joining streams with other data sources (e.g., tables or databases) to add context or additional details to the original data.

### Error Handling

Kafka Connect provides error reporting to handle errors encountered at various stages of processing. By default, any error during conversion or transformations causes the connector to fail. Each connector configuration can also enable tolerating such errors by skipping them and optionally writing each error and details of the failed operation and problematic record to the Connect application log.

To report errors within a connector's converter, transforms, or within the sink connector itself to the log, set `errors.log.enable=true` in the connector configuration to log details of each error and the problem record's topic, partition, and offset. For additional debugging purposes, set `errors.log.include.messages=true` to also log the problem record key, value, and headers (note this may log sensitive information).

To report errors within a connector's converter, transforms, or within the sink connector itself to a dead letter queue topic, set `errors.deadletterqueue.topic.name`, and optionally `errors.deadletterqueue.context.headers.enable=true`.
