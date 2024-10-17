## Question 21:
A media company streams live video content, which generates logs of viewer interactions (e.g., play, pause, stop) in real-time. To enhance viewer experience through personalized content and advertisements, they need to analyze these logs in real-time. The logs are stored in NoSQL databases across different geographical locations. Considering the need for low-latency analysis, which setup is most appropriate?

1. Use Kafka Connect with a custom NoSQL Source Connector for each geographical location to ingest logs into Kafka, then utilize Kafka Streams for real-time analysis and dynamic content delivery.
2. Directly stream logs from NoSQL databases to Kafka using log-based Change Data Capture (CDC) connectors specific to each NoSQL database type, followed by processing with ksqlDB to generate viewer insights and personalized content recommendations.
3. Configure a network of MQTT brokers to collect logs from each location, and then use an MQTT Source Connector to consolidate logs into Kafka. Apply a Kafka Streams application to analyze viewer interactions and adjust content recommendations.
4. Implement a batch ETL process to extract logs from NoSQL databases nightly, load them into Kafka for next-day processing with Kafka Streams, and update content recommendations based on the analysis.

**Response:**

The correct answer is **2. Directly stream logs from NoSQL databases to Kafka using log-based Change Data Capture (CDC) connectors specific to each NoSQL database type, followed by processing with ksqlDB to generate viewer insights and personalized content recommendations.**

**Explanation:**
- **1.** Custom connectors for each location could work but might not offer the low-latency needed for real-time analysis and personalized content delivery compared to CDC connectors that capture changes as they happen.
- **2.** Correct. CDC connectors are designed to capture and stream real-time changes (including viewer interactions) from databases to Kafka, facilitating immediate analysis. Using ksqlDB allows for leveraging SQL-like queries for real-time data processing, ideal for generating insights and recommendations with minimal latency.
- **3.** While MQTT is suitable for IoT data, using it for log data from NoSQL databases introduces unnecessary complexity and may not provide the direct integration or the efficiency of CDC connectors.
- **4.** Batch ETL processes cannot meet the requirements for real-time analysis and dynamic content personalization due to the inherent delay in processing.

## Question 22:
An online retailer integrates user reviews from their website into Kafka to perform sentiment analysis and adjust product rankings accordingly. Reviews are initially posted to a MongoDB database. To ensure the analysis reflects recent feedback, which configuration ensures the most efficient and timely data flow into Kafka?

1. Configure MongoDB Source Connector to capture new and updated reviews into Kafka, then use Kafka Streams for sentiment analysis and to adjust product rankings in near-real-time.
2. Utilize a cron job to export reviews from MongoDB to CSV files at regular intervals, then use File Source Connectors to ingest these files into Kafka, followed by processing with Kafka Streams.
3. Deploy log-based CDC connectors to stream only the changes (new and updated reviews) from MongoDB into Kafka, leveraging ksqlDB for continuous sentiment analysis and product ranking adjustments.
4. Directly access the MongoDB API from Kafka Streams applications to fetch new and updated reviews, perform sentiment analysis, and update product rankings without storing reviews in Kafka.

**Response:**

The correct answer is **3. Deploy log-based CDC connectors to stream only the changes (new and updated reviews) from MongoDB into Kafka, leveraging ksqlDB for continuous sentiment analysis and product ranking adjustments.**

**Explanation:**
- **1.** MongoDB Source Connector can efficiently ingest data into Kafka, but this option doesn't specify the efficiency of capturing only new or updated reviews like CDC would.
- **2.** Exporting to CSV and using File Source Connectors introduces delays, making it unsuitable for reflecting recent feedback in near-real-time.
- **3.** Correct. CDC connectors are specifically designed for efficient, real-time data synchronization by capturing only new and modified data. Using ksqlDB for sentiment analysis allows for real-time processing and immediate action on the insights.
- **4.** Fetching data directly via the MongoDB API bypasses Kafka's benefits of decoupling data producers and consumers, scalability, and fault tolerance, making it less efficient for real-time analysis and adjustment of product rankings.

## Question 23:
A financial institution aims to merge transaction data from legacy systems with real-time fraud detection models running on Kafka Streams. The transaction data resides in various legacy databases and must be enriched with real-time fraud signals before being presented on a dashboard. What's the most effective architecture for this use case?

1. Use JDBC Source Connectors to ingest transaction data from legacy databases into Kafka. Enrich the data using Kafka Streams by joining it with real-time fraud detection signals. Utilize a Kafka Connect Sink Connector to publish the enriched data to the dashboard.
2. Implement custom Kafka Producers to extract transaction data from legacy systems, enriching the data in-flight with fraud detection signals before producing it to a Kafka topic for dashboard consumption.
3. Directly connect the legacy databases to Kafka Streams applications using custom database clients. Perform the enrichment with real-time fraud detection signals in the application, then produce the enriched data to a Kafka topic for dashboard visualization.
4. Leverage Change Data Capture (CDC) connectors to stream transaction data from legacy databases into Kafka. Use Kafka Streams for real-time enrichment with fraud detection signals and ksqlDB to further process and prepare the data for dashboard presentation.

**Response:**

The correct answer is **4. Leverage Change Data Capture (CDC) connectors to stream transaction data from legacy databases into Kafka. Use Kafka Streams for real-time enrichment with fraud detection signals and ksqlDB to further process and prepare the data for dashboard presentation.**

**Explanation:**
- **1.** While using JDBC Source Connectors to ingest transaction data into Kafka is a viable option, it may not provide the low-latency required for real-time fraud detection compared to the efficiency of CDC connectors.
- **2.** Implementing custom Kafka Producers requires significant development effort and might not be as efficient or scalable as using built-in Kafka Connect connectors. Additionally, enriching data in-flight before it reaches Kafka can complicate the architecture.
- **3.** Connecting legacy databases directly to Kafka Streams applications complicates the architecture and increases the coupling between data sources and processing logic, making the system less resilient and scalable.
- **4.** Correct. CDC connectors are ideal for capturing changes from legacy databases in real-time, minimizing latency. Kafka Streams enables sophisticated stream processing capabilities for enrichment with fraud detection signals. Using ksqlDB allows for additional processing and preparation of the enriched data in a more accessible SQL-like language, making it suitable for dynamic dashboard presentation. This option efficiently combines real-time data integration, processing, and presentation while leveraging the strengths of the Kafka ecosystem.

## Question 24

Where are the Kafka Connect connector configurations stored?

- 1. In a separate config file on each Kafka Connect worker
- 2. In the Kafka broker's config directory
- 3. In Zookeeper under the `/kafka-connect` znode
- 4. In a special Kafka topic named `connect-configs`

**Answer:** 4

**Explanation:**
Kafka Connect uses a special Kafka topic named `connect-configs` to store connector and task configurations. When a connector is created or updated, the configurations are persisted in this topic.

- 1 is incorrect because connector configs are not stored in separate files on the worker nodes.
- 2 is incorrect as connector configs are not stored in the Kafka broker's config directory.
- 3 is incorrect because while Kafka Connect uses Zookeeper for some coordination tasks, connector configs specifically are not stored in Zookeeper.

## Question 25

You want to use Kafka Connect to export data from a Kafka topic to a relational database. Which type of connector should you use?

- 1. Source Connector
- 2. Sink Connector
- 3. Transformation Connector
- 4. Import Connector

**Answer:** 2

**Explanation:**
In Kafka Connect, a Sink Connector is used to consume data from Kafka topics and deliver it to an external system, such as a relational database, a search index, or a file system.

- 1 Source Connector is used to import data from an external system into Kafka topics, which is the opposite of what's needed here.
- 3 is incorrect because Transformation Connectors are not a real type in Kafka Connect. Transformations can be applied to a connector configuration, but they are not a separate connector type.
- 4 is incorrect because "Import Connector" is not a real term in Kafka Connect.

## Question 26

You need to stream data from a Twitter feed into a Kafka topic for real-time processing. Which Kafka Connect connector type is most appropriate?

- 1. Sink Connector
- 2. Source Connector
- 3. Transformation Connector
- 4. Export Connector

**Answer:** 2

**Explanation:**
A Kafka Connect Source Connector is used to import data from an external source, such as a database, a Twitter feed, or a messaging system, and publish that data to Kafka topics.

- 1 Sink Connector is used to export data from Kafka topics to an external system, which is the opposite of what's needed here.
- 3 is incorrect because Transformation Connectors are not a real type in Kafka Connect. Transformations can be applied to a connector configuration, but they are not a separate connector type.
- 4 is incorrect because "Export Connector" is not a real term in Kafka Connect.

## Question 27

You are using Kafka Connect to move data from a source system into Kafka for real-time processing with Kafka Streams. After processing, the results need to be stored in HDFS for batch analysis. Which combination of connector types will you need?

- 1. Source Connector -> Sink Connector
- 2. Sink Connector -> Source Connector
- 3. Source Connector -> Source Connector
- 4. Sink Connector -> Sink Connector

**Answer:** 1

**Explanation:**
This scenario requires a combination of a Source Connector and a Sink Connector:

1. A Source Connector is needed to import data from the source system into Kafka topics.
2. Kafka Streams can then process this data in real-time.
3. Finally, a Sink Connector is needed to export the processed results from Kafka topics to HDFS.

The other options are incorrect:

- 2 would be importing from Kafka to the source system and then from HDFS to Kafka, which is the wrong direction.
- 3 and 4 use the same connector type twice, which doesn't make sense for moving data from a source to Kafka to a sink.

## Question 28

You are using a JDBC source connector to copy data from a database table to a Kafka topic. The table has 5 columns. How many tasks will be created by the connector?

- 1. 1
- 2. 5
- 3. It depends on the `max.tasks` configuration of the connector
- 4. It depends on the number of partitions in the Kafka topic

**Answer:** 1

**Explanation:**
When using a JDBC source connector to copy data from a database table to a Kafka topic, the number of tasks created by the connector is not directly related to the number of columns in the table.

- 2. default, a JDBC source connector creates only one task per table, regardless of the number of columns. Each task reads data from the entire table and writes it to the Kafka topic.

The number of tasks can be controlled by the `max.tasks` configuration property of the connector. However, even if `max.tasks` is set to a value greater than 1, the JDBC connector will still create only one task per table.

The reason for this is that the JDBC connector reads data from the table sequentially, and splitting the data across multiple tasks based on columns would not provide any parallelism benefits.

Statement 2 is incorrect because the number of tasks is not determined by the number of columns in the table.

Statement 3 is partially correct, but it doesn't apply to the JDBC connector specifically. The `max.tasks` configuration is used by some connectors to control the maximum number of tasks, but the JDBC connector always creates one task per table.

Statement 4 is incorrect because the number of tasks is not related to the number of partitions in the Kafka topic. The JDBC connector's task reads data from the table and writes it to the topic, regardless of the topic's partitioning.

## Question 29

What happens if the `max.tasks` configuration is set to a value less than the number of tables being copied by a JDBC source connector?

1. The connector will create one task per table, ignoring the `max.tasks` setting
2. The connector will create tasks up to the `max.tasks` limit, potentially leaving some tables without dedicated tasks
3. The connector will distribute the tables evenly among the available tasks
4. The connector will fail with an error due to the insufficient number of tasks

**Answer:** 3

**Explanation:**
When the `max.tasks` configuration is set to a value less than the number of tables being copied by a JDBC source connector, the connector will distribute the tables evenly among the available tasks.

Key points:
- The connector respects the `max.tasks` setting and creates up to that number of tasks.
- All tables are processed; no tables are left without tasks.
- Tables are assigned to tasks in a way that balances the load.
- Each task will handle multiple tables, processing them sequentially.

Example:
If there are 10 tables and `max.tasks` is set to 5:
- The connector creates 5 tasks.
- Each task is assigned 2 tables.
- All tables are processed, and the workload is distributed evenly among the tasks.

This approach ensures that all data is ingested into Kafka, maintaining data integrity and completeness while respecting the `max.tasks` limit.

## Question 30

How can you increase the parallelism of a JDBC source connector to improve the performance of copying data from a database to Kafka?

1. Increase the `max.tasks` configuration of the connector
2. Increase the number of partitions in the target Kafka topic
3. Increase the `tasks.max` configuration of the Kafka Connect workers
4. Use multiple instances of the JDBC connector, each copying a different subset of tables

**Answer:** 1 and 4

**Explanation:**
To increase the parallelism of a JDBC source connector and improve performance, you can use a combination of approaches:

1. Increase the `max.tasks` configuration of the connector:
   - This allows the connector to create more tasks within a single instance.
   - Each task can process data independently, increasing parallelism.

4. Use multiple instances of the JDBC connector, each copying a different subset of tables:
   - Distributes the workload across multiple connector instances.
   - Each instance can run tasks in parallel, further enhancing performance.

Combining both options can maximize parallelism and performance, although it's important to manage the complexity that comes with multiple connector instances.

Additional notes:
- Option 2 (increasing partitions) can help with downstream consumer parallelism but doesn't directly affect the connector's parallelism.
- Option 3 (increasing `tasks.max` of Kafka Connect workers) allows the cluster to handle more tasks but doesn't increase the connector's parallelism unless combined with Option 1.