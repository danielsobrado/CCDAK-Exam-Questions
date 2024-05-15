### Question 21:
A media company streams live video content, which generates logs of viewer interactions (e.g., play, pause, stop) in real-time. To enhance viewer experience through personalized content and advertisements, they need to analyze these logs in real-time. The logs are stored in NoSQL databases across different geographical locations. Considering the need for low-latency analysis, which setup is most appropriate?

a. Use Kafka Connect with a custom NoSQL Source Connector for each geographical location to ingest logs into Kafka, then utilize Kafka Streams for real-time analysis and dynamic content delivery.
b. Directly stream logs from NoSQL databases to Kafka using log-based Change Data Capture (CDC) connectors specific to each NoSQL database type, followed by processing with ksqlDB to generate viewer insights and personalized content recommendations.
c. Configure a network of MQTT brokers to collect logs from each location, and then use an MQTT Source Connector to consolidate logs into Kafka. Apply a Kafka Streams application to analyze viewer interactions and adjust content recommendations.
d. Implement a batch ETL process to extract logs from NoSQL databases nightly, load them into Kafka for next-day processing with Kafka Streams, and update content recommendations based on the analysis.

**Response:**

The correct answer is **b. Directly stream logs from NoSQL databases to Kafka using log-based Change Data Capture (CDC) connectors specific to each NoSQL database type, followed by processing with ksqlDB to generate viewer insights and personalized content recommendations.**

**Explanation:**
- **a.** Custom connectors for each location could work but might not offer the low-latency needed for real-time analysis and personalized content delivery compared to CDC connectors that capture changes as they happen.
- **b.** Correct. CDC connectors are designed to capture and stream real-time changes (including viewer interactions) from databases to Kafka, facilitating immediate analysis. Using ksqlDB allows for leveraging SQL-like queries for real-time data processing, ideal for generating insights and recommendations with minimal latency.
- **c.** While MQTT is suitable for IoT data, using it for log data from NoSQL databases introduces unnecessary complexity and may not provide the direct integration or the efficiency of CDC connectors.
- **d.** Batch ETL processes cannot meet the requirements for real-time analysis and dynamic content personalization due to the inherent delay in processing.

### Question 22:
An online retailer integrates user reviews from their website into Kafka to perform sentiment analysis and adjust product rankings accordingly. Reviews are initially posted to a MongoDB database. To ensure the analysis reflects recent feedback, which configuration ensures the most efficient and timely data flow into Kafka?

a. Configure MongoDB Source Connector to capture new and updated reviews into Kafka, then use Kafka Streams for sentiment analysis and to adjust product rankings in near-real-time.
b. Utilize a cron job to export reviews from MongoDB to CSV files at regular intervals, then use File Source Connectors to ingest these files into Kafka, followed by processing with Kafka Streams.
c. Deploy log-based CDC connectors to stream only the changes (new and updated reviews) from MongoDB into Kafka, leveraging ksqlDB for continuous sentiment analysis and product ranking adjustments.
d. Directly access the MongoDB API from Kafka Streams applications to fetch new and updated reviews, perform sentiment analysis, and update product rankings without storing reviews in Kafka.

**Response:**

The correct answer is **c. Deploy log-based CDC connectors to stream only the changes (new and updated reviews) from MongoDB into Kafka, leveraging ksqlDB for continuous sentiment analysis and product ranking adjustments.**

**Explanation:**
- **a.** MongoDB Source Connector can efficiently ingest data into Kafka, but this option doesn't specify the efficiency of capturing only new or updated reviews like CDC would.
- **b.** Exporting to CSV and using File Source Connectors introduces delays, making it unsuitable for reflecting recent feedback in near-real-time.
- **c.** Correct. CDC connectors are specifically designed for efficient, real-time data synchronization by capturing only new and modified data. Using ksqlDB for sentiment analysis allows for real-time processing and immediate action on the insights.
- **d.** Fetching data directly via the MongoDB API bypasses Kafka's benefits of decoupling data producers and consumers, scalability, and fault tolerance, making it less efficient for real-time analysis and adjustment of product rankings.

### Question 23:
A financial institution aims to merge transaction data from legacy systems with real-time fraud detection models running on Kafka Streams. The transaction data resides in various legacy databases and must be enriched with real-time fraud signals before being presented on a dashboard. What's the most effective architecture for this use case?

a. Use JDBC Source Connectors to ingest transaction data from legacy databases into Kafka. Enrich the data using Kafka Streams by joining it with real-time fraud detection signals. Utilize a Kafka Connect Sink Connector to publish the enriched data to the dashboard.
b. Implement custom Kafka Producers to extract transaction data from legacy systems, enriching the data in-flight with fraud detection signals before producing it to a Kafka topic for dashboard consumption.
c. Directly connect the legacy databases to Kafka Streams applications using custom database clients. Perform the enrichment with real-time fraud detection signals in the application, then produce the enriched data to a Kafka topic for dashboard visualization.
d. Leverage Change Data Capture (CDC) connectors to stream transaction data from legacy databases into Kafka. Use Kafka Streams for real-time enrichment with fraud detection signals and ksqlDB to further process and prepare the data for dashboard presentation.

**Response:**

The correct answer is **d. Leverage Change Data Capture (CDC) connectors to stream transaction data from legacy databases into Kafka. Use Kafka Streams for real-time enrichment with fraud detection signals and ksqlDB to further process and prepare the data for dashboard presentation.**

**Explanation:**
- **a.** While using JDBC Source Connectors to ingest transaction data into Kafka is a viable option, it may not provide the low-latency required for real-time fraud detection compared to the efficiency of CDC connectors.
- **b.** Implementing custom Kafka Producers requires significant development effort and might not be as efficient or scalable as using built-in Kafka Connect connectors. Additionally, enriching data in-flight before it reaches Kafka can complicate the architecture.
- **c.** Connecting legacy databases directly to Kafka Streams applications complicates the architecture and increases the coupling between data sources and processing logic, making the system less resilient and scalable.
- **d.** Correct. CDC connectors are ideal for capturing changes from legacy databases in real-time, minimizing latency. Kafka Streams enables sophisticated stream processing capabilities for enrichment with fraud detection signals. Using ksqlDB allows for additional processing and preparation of the enriched data in a more accessible SQL-like language, making it suitable for dynamic dashboard presentation. This option efficiently combines real-time data integration, processing, and presentation while leveraging the strengths of the Kafka ecosystem.

## Question 24

Where are the Kafka Connect connector configurations stored?

- A. In a separate config file on each Kafka Connect worker
- B. In the Kafka broker's config directory
- C. In Zookeeper under the `/kafka-connect` znode
- D. In a special Kafka topic named `connect-configs`

**Answer:** D

**Explanation:**
Kafka Connect uses a special Kafka topic named `connect-configs` to store connector and task configurations. When a connector is created or updated, the configurations are persisted in this topic.

- A is incorrect because connector configs are not stored in separate files on the worker nodes.
- B is incorrect as connector configs are not stored in the Kafka broker's config directory.
- C is incorrect because while Kafka Connect uses Zookeeper for some coordination tasks, connector configs specifically are not stored in Zookeeper.

## Question 25

You want to use Kafka Connect to export data from a Kafka topic to a relational database. Which type of connector should you use?

- A. Source Connector
- B. Sink Connector
- C. Transformation Connector
- D. Import Connector

**Answer:** B

**Explanation:**
In Kafka Connect, a Sink Connector is used to consume data from Kafka topics and deliver it to an external system, such as a relational database, a search index, or a file system.

- A Source Connector is used to import data from an external system into Kafka topics, which is the opposite of what's needed here.
- C is incorrect because Transformation Connectors are not a real type in Kafka Connect. Transformations can be applied to a connector configuration, but they are not a separate connector type.
- D is incorrect because "Import Connector" is not a real term in Kafka Connect.

## Question 26

You need to stream data from a Twitter feed into a Kafka topic for real-time processing. Which Kafka Connect connector type is most appropriate?

- A. Sink Connector
- B. Source Connector
- C. Transformation Connector
- D. Export Connector

**Answer:** B

**Explanation:**
A Kafka Connect Source Connector is used to import data from an external source, such as a database, a Twitter feed, or a messaging system, and publish that data to Kafka topics.

- A Sink Connector is used to export data from Kafka topics to an external system, which is the opposite of what's needed here.
- C is incorrect because Transformation Connectors are not a real type in Kafka Connect. Transformations can be applied to a connector configuration, but they are not a separate connector type.
- D is incorrect because "Export Connector" is not a real term in Kafka Connect.

## Question 27

You are using Kafka Connect to move data from a source system into Kafka for real-time processing with Kafka Streams. After processing, the results need to be stored in HDFS for batch analysis. Which combination of connector types will you need?

- A. Source Connector -> Sink Connector
- B. Sink Connector -> Source Connector
- C. Source Connector -> Source Connector
- D. Sink Connector -> Sink Connector

**Answer:** A

**Explanation:**
This scenario requires a combination of a Source Connector and a Sink Connector:

1. A Source Connector is needed to import data from the source system into Kafka topics.
2. Kafka Streams can then process this data in real-time.
3. Finally, a Sink Connector is needed to export the processed results from Kafka topics to HDFS.

The other options are incorrect:

- B would be importing from Kafka to the source system and then from HDFS to Kafka, which is the wrong direction.
- C and D use the same connector type twice, which doesn't make sense for moving data from a source to Kafka to a sink.

## Question 28

You are using a JDBC source connector to copy data from a database table to a Kafka topic. The table has 5 columns. How many tasks will be created by the connector?

- A. 1
- B. 5
- C. It depends on the `max.tasks` configuration of the connector
- D. It depends on the number of partitions in the Kafka topic

**Answer:** A

**Explanation:**
When using a JDBC source connector to copy data from a database table to a Kafka topic, the number of tasks created by the connector is not directly related to the number of columns in the table.

By default, a JDBC source connector creates only one task per table, regardless of the number of columns. Each task reads data from the entire table and writes it to the Kafka topic.

The number of tasks can be controlled by the `max.tasks` configuration property of the connector. However, even if `max.tasks` is set to a value greater than 1, the JDBC connector will still create only one task per table.

The reason for this is that the JDBC connector reads data from the table sequentially, and splitting the data across multiple tasks based on columns would not provide any parallelism benefits.

Statement B is incorrect because the number of tasks is not determined by the number of columns in the table.

Statement C is partially correct, but it doesn't apply to the JDBC connector specifically. The `max.tasks` configuration is used by some connectors to control the maximum number of tasks, but the JDBC connector always creates one task per table.

Statement D is incorrect because the number of tasks is not related to the number of partitions in the Kafka topic. The JDBC connector's task reads data from the table and writes it to the topic, regardless of the topic's partitioning.

## Question 29

What happens if the `max.tasks` configuration is set to a value less than the number of tables being copied by a JDBC source connector?

- A. The connector will create one task per table, ignoring the `max.tasks` setting
- B. The connector will create tasks only for a subset of the tables, up to the `max.tasks` limit
- C. The connector will distribute the tables evenly among the available tasks
- D. The connector will fail with an error due to the insufficient number of tasks

**Answer:** B

**Explanation:**
When the `max.tasks` configuration is set to a value less than the number of tables being copied by a JDBC source connector, the connector will create tasks only for a subset of the tables, up to the `max.tasks` limit.

The JDBC connector assigns tables to tasks based on the `max.tasks` configuration. If `max.tasks` is less than the number of tables, the connector will prioritize some tables over others.

Here's how it works:

1. The connector retrieves the list of tables to be copied based on the connector's configuration.
2. If the number of tables is less than or equal to `max.tasks`, the connector creates one task per table.
3. If the number of tables exceeds `max.tasks`, the connector selects a subset of tables up to the `max.tasks` limit and creates tasks only for those tables.
4. The remaining tables will not have dedicated tasks and will not be copied.

The specific subset of tables chosen when `max.tasks` is exceeded depends on the connector's implementation and may vary based on factors such as table order or naming.

Statement A is incorrect because the connector does not ignore the `max.tasks` setting. It will create tasks only up to the specified limit.

Statement C is incorrect because the connector does not distribute tables evenly among tasks. Each task is assigned to a specific table.

Statement D is incorrect because the connector does not fail with an error when `max.tasks` is less than the number of tables. It will proceed with copying a subset of the tables.

## Question 30

How can you increase the parallelism of a JDBC source connector to improve the performance of copying data from a database to Kafka?

- A. Increase the `max.tasks` configuration of the connector
- B. Increase the number of partitions in the target Kafka topic
- C. Increase the `tasks.max` configuration of the Kafka Connect workers
- D. Use multiple instances of the JDBC connector, each copying a different subset of tables

**Answer:** D

**Explanation:**
To increase the parallelism of a JDBC source connector and improve the performance of copying data from a database to Kafka, you can use multiple instances of the JDBC connector, each copying a different subset of tables.

The JDBC connector creates one task per table, and each task reads data from the table sequentially. Increasing the `max.tasks` configuration or the number of partitions in the target Kafka topic will not provide any parallelism benefits for a single JDBC connector instance.

Instead, you can deploy multiple instances of the JDBC connector, each configured to copy a different subset of tables. This way, the tables will be distributed among the connector instances, and each instance will have its own set of tasks reading data in parallel.

Here's how you can achieve this:

1. Create multiple instances of the JDBC connector in your Kafka Connect cluster.
2. Configure each connector instance to copy a specific subset of tables by specifying the table names or a pattern in the connector's configuration.
3. Ensure that the subsets of tables assigned to each connector instance do not overlap to avoid duplicate data copying.
4. Start all the connector instances, and they will work in parallel, each copying its assigned subset of tables to Kafka.

By distributing the tables among multiple connector instances, you can leverage the parallelism of the Kafka Connect framework and improve the overall throughput of data copying from the database to Kafka.

Statement A is incorrect because increasing `max.tasks` will not provide parallelism benefits for a single JDBC connector instance. The connector will still create one task per table.

Statement B is incorrect because increasing the number of partitions in the target Kafka topic does not affect the parallelism of the JDBC connector. The connector's tasks write data to the topic sequentially.

Statement C is incorrect because increasing `tasks.max` of the Kafka Connect workers will not automatically increase the parallelism of the JDBC connector. The connector's parallelism is determined by the number of tasks it creates based on the number of tables.

