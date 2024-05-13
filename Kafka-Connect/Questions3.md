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

A. In a separate config file on each Kafka Connect worker
B. In the Kafka broker's config directory
C. In Zookeeper under the `/kafka-connect` znode
D. In a special Kafka topic named `connect-configs`

**Answer:** D

**Explanation:**
Kafka Connect uses a special Kafka topic named `connect-configs` to store connector and task configurations. When a connector is created or updated, the configurations are persisted in this topic.

- A is incorrect because connector configs are not stored in separate files on the worker nodes.
- B is incorrect as connector configs are not stored in the Kafka broker's config directory.
- C is incorrect because while Kafka Connect uses Zookeeper for some coordination tasks, connector configs specifically are not stored in Zookeeper.

## Question 25

You want to use Kafka Connect to export data from a Kafka topic to a relational database. Which type of connector should you use?

A. Source Connector
B. Sink Connector
C. Transformation Connector
D. Import Connector

**Answer:** B

**Explanation:**
In Kafka Connect, a Sink Connector is used to consume data from Kafka topics and deliver it to an external system, such as a relational database, a search index, or a file system.

- A Source Connector is used to import data from an external system into Kafka topics, which is the opposite of what's needed here.
- C is incorrect because Transformation Connectors are not a real type in Kafka Connect. Transformations can be applied to a connector configuration, but they are not a separate connector type.
- D is incorrect because "Import Connector" is not a real term in Kafka Connect.

## Question 26

You need to stream data from a Twitter feed into a Kafka topic for real-time processing. Which Kafka Connect connector type is most appropriate?

A. Sink Connector
B. Source Connector
C. Transformation Connector
D. Export Connector

**Answer:** B

**Explanation:**
A Kafka Connect Source Connector is used to import data from an external source, such as a database, a Twitter feed, or a messaging system, and publish that data to Kafka topics.

- A Sink Connector is used to export data from Kafka topics to an external system, which is the opposite of what's needed here.
- C is incorrect because Transformation Connectors are not a real type in Kafka Connect. Transformations can be applied to a connector configuration, but they are not a separate connector type.
- D is incorrect because "Export Connector" is not a real term in Kafka Connect.

## Question 27

You are using Kafka Connect to move data from a source system into Kafka for real-time processing with Kafka Streams. After processing, the results need to be stored in HDFS for batch analysis. Which combination of connector types will you need?

A. Source Connector -> Sink Connector
B. Sink Connector -> Source Connector
C. Source Connector -> Source Connector
D. Sink Connector -> Sink Connector

**Answer:** A

**Explanation:**
This scenario requires a combination of a Source Connector and a Sink Connector:

1. A Source Connector is needed to import data from the source system into Kafka topics.
2. Kafka Streams can then process this data in real-time.
3. Finally, a Sink Connector is needed to export the processed results from Kafka topics to HDFS.

The other options are incorrect:

- B would be importing from Kafka to the source system and then from HDFS to Kafka, which is the wrong direction.
- C and D use the same connector type twice, which doesn't make sense for moving data from a source to Kafka to a sink.
