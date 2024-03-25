### Question 1:
For a system designed to read data from an external database, perform some transformations, and then store the results in a Kafka topic, which approach is most suitable?

1. Consumer + Producer
2. Kafka Connect Source
3. Kafka Connect Sink
4. Kafka Streams

**Response:**

The correct answer is **2. Kafka Connect Source.**

**Explanation:**
Kafka Connect Source is designed for importing data from external systems into Kafka topics. It can easily read from an external database, and with the appropriate connectors and transformations configured, it can modify the data as required before making it available in a Kafka topic.

### Question 2:
When needing to aggregate real-time data from a Kafka topic, compute running totals, and then publish those totals back to another Kafka topic for further analysis, which tool should you use?

1. Consumer + Producer
2. Kafka Connect Source
3. Kafka Connect Sink
4. Kafka Streams

**Response:**

The correct answer is **4. Kafka Streams.**

**Explanation:**
Kafka Streams is specifically designed for building real-time streaming data pipelines and applications that transform or react to the streams of data. For tasks like aggregating data and computing running totals, Kafka Streams provides the necessary stateful operations and can directly publish the results back to another Kafka topic.

### Question 3:
If the objective is to periodically export data from a Kafka topic to a relational database for long-term storage and analysis, which Kafka component would best fulfill this requirement?

1. Consumer + Producer
2. Kafka Connect Source
3. Kafka Connect Sink
4. Kafka Streams

**Response:**

The correct answer is **3. Kafka Connect Sink.**

**Explanation:**
Kafka Connect Sink is designed to export data from Kafka topics into external systems such as databases, key-value stores, search indexes, and file systems. For scenarios where the goal is to move data from Kafka to a relational database, a Kafka Connect Sink connector can be configured to handle this task efficiently and with minimal coding effort.

### Question 4:
A financial institution wants to analyze transaction data in real-time to detect fraudulent activities. The transaction data, which includes sensitive information, is initially stored in a mainframe system. Which approach ensures secure, real-time analysis of this data by a Kafka Streams application while complying with data privacy regulations?

a. Utilize a mainframe connector with Kafka Connect to ingest the transaction data into a Kafka topic. Apply a Kafka Streams application to anonymize sensitive information in the stream before conducting fraud analysis.

b. Directly connect the mainframe system to the Kafka Streams application using a custom API, ensuring sensitive data is filtered out during the streaming process.

c. Employ ksqlDB to directly query the mainframe system, apply data anonymization functions to filter out sensitive information, and then write the sanitized data to a Kafka topic for stream processing.

d. Implement a batch process to extract transaction data periodically from the mainframe, cleanse the data of sensitive information, and then load the sanitized data into Kafka topics for streaming analysis.

**Response:**

The correct answer is **a. Utilize a mainframe connector with Kafka Connect to ingest the transaction data into a Kafka topic. Apply a Kafka Streams application to anonymize sensitive information in the stream before conducting fraud analysis.**

### Question 5:
Your organization aims to implement a real-time recommendation engine for e-commerce users based on their browsing behavior. User session data is considered sensitive and must be anonymized before processing. How can the Kafka ecosystem be leveraged to meet these requirements?

a. Deploy a Kafka Connect Source Connector to capture session data directly into Kafka, using Stream Processing to anonymize user identifiers before aggregating sessions for recommendation analysis.

b. Use a custom Kafka Producer application to publish session data to a topic, applying a Stream Processor to anonymize and then process the data for generating recommendations.

c. Implement a Kafka Connect Sink Connector to store session data into a NoSQL database, with a pre-processor to remove sensitive information before ingestion. Use Kafka Streams to read from the database for analysis.

d. Create a ksqlDB process to pull session data from source systems, apply anonymization functions within ksqlDB, and output the clean data to a Kafka topic for further processing by the Kafka Streams application.

**Response:**

The correct answer is **a. Deploy a Kafka Connect Source Connector to capture session data directly into Kafka, using Stream Processing to anonymize user identifiers before aggregating sessions for recommendation analysis.**

### Question 6:
An organization is implementing a system to monitor and alert on infrastructure health status in real-time. The system collects metrics from various sources, including some that contain proprietary information. Which approach ensures that only non-proprietary, critical health metrics are analyzed and alerted on?

a. Use Kafka Connect with appropriate Source Connectors for each metric source, configuring the connectors to filter out proprietary information. Process the filtered metrics stream with Kafka Streams for alerting.

b. Directly stream all metrics into Kafka using custom Producers, then employ a Kafka Streams application to separate proprietary data from non-proprietary data, and analyze the latter for alerting.

c. Implement a series of ksqlDB statements to ingest metrics into Kafka, applying filtering logic within ksqlDB to remove proprietary information before streaming processing and alerting.

d. Configure a Kafka Connect Sink Connector to aggregate all metrics into a centralized database, followed by batch processing to remove proprietary information before streaming the data into Kafka for real-time analysis.

**Response:**

The correct answer is **a. Use Kafka Connect with appropriate Source Connectors for each metric source, configuring the connectors to filter out proprietary information. Process the filtered metrics stream with Kafka Streams for alerting.**

### Question 7:
A multinational corporation is looking to aggregate sales data from multiple regional systems into a centralized Kafka topic for real-time analysis and reporting. The regional systems vary in technology, including SQL databases and cloud-based storage solutions. Which solution enables the efficient and unified ingestion of these diverse data sources into Kafka?

a. Deploy Kafka Streams applications near each regional system to collect and forward data to the centralized Kafka topic.
b. Use Kafka Connect with a mix of Source Connectors suitable for each regional system's technology to ingest data directly into Kafka.
c. Implement custom Kafka Producers embedded within each regional system to push data to the centralized Kafka topic.
d. Configure a Kafka Connect Sink Connector for each regional system to replicate data into the centralized Kafka topic.

**Response:**

The correct answer is **b. Use Kafka Connect with a mix of Source Connectors suitable for each regional system's technology to ingest data directly into Kafka.**

### Question 8:
An online media platform wishes to analyze user interactions (clicks, views, and comments) in real-time to dynamically adjust content recommendations. The platform generates a high volume of interaction data, necessitating scalable and real-time processing. What architecture best suits this requirement?

a. Utilize a Kafka Connect Source Connector to ingest interaction data into Kafka, then process this data with Kafka Streams to update content recommendations in real-time.
b. Directly stream interaction data into Kafka using a custom API, then use ksqlDB to perform real-time analysis and generate content recommendations.
c. Implement batch processing jobs to periodically analyze interaction data stored in an external database, and then use Kafka to distribute batch analysis results for content recommendation updates.
d. Configure Kafka Connect Sink Connectors to collect interaction data into a big data platform first, then process the data using external stream processing tools before updating content recommendations.

**Response:**

The correct answer is **a. Utilize a Kafka Connect Source Connector to ingest interaction data into Kafka, then process this data with Kafka Streams to update content recommendations in real-time.**

### Question 9:
A utility company monitors a network of IoT devices deployed across an energy grid. The devices send telemetry data (e.g., power usage, system health) every minute. The company wants to aggregate this data for near-real-time monitoring and anomaly detection. Which Kafka-based solution efficiently achieves this goal?

a. Configure Kafka Connect Sink Connectors to collect telemetry data from the IoT devices into Kafka, followed by a Kafka Streams application to aggregate and analyze the data.
b. Use Kafka Connect Source Connectors appropriate for the IoT devices' communication protocols to ingest telemetry data into Kafka, then employ Kafka Streams for data aggregation and anomaly detection.
c. Develop custom Kafka Producers within the IoT devices to send data directly to Kafka topics, then use external tools to pull and analyze the data from Kafka.
d. Implement a centralized database to collect IoT telemetry data first, then use Kafka Connect Source Connectors to ingest the data from the database into Kafka for further processing.

**Response:**

The correct answer is **b. Use Kafka Connect Source Connectors appropriate for the IoT devices' communication protocols to ingest telemetry data into Kafka, then employ Kafka Streams for data aggregation and anomaly detection.**

### Question 10:
A digital marketing platform analyzes user activities to send personalized marketing emails. The platform uses Kafka to stream activity data, with fluctuations in data volume throughout the day. To ensure optimal performance during peak data inflow, what strategy should be employed?

a. Dynamically adjust the number of partitions in the user activities topic based on incoming data volume.
b. Scale the Kafka Streams application instances up or down in response to the processing load.
c. Increase or decrease the number of Kafka Connect Source Connector tasks to match the rate of incoming user activity data.
d. Modify the replication factor of the user activities topic during high load periods to improve data durability and availability.

**Response:**

The correct answer is **b. Scale the Kafka Streams application instances up or down in response to the processing load.**

### Question 11:
An e-commerce company uses Kafka to process customer orders. During sales events, the order volume spikes significantly. Which approach ensures the system scales efficiently to handle these spikes in order volume?

a. Automatically adjust the number of topics to spread the increased order messages across more Kafka topics during sales events.
b. Use Kafka Connect with scalable Source Connectors to adjust the throughput based on order volume.
c. Scale out the Kafka broker cluster by adding more brokers during high-volume periods and scale in when the volume decreases.
d. Configure the Kafka producer to dynamically adjust batch size and linger time based on the current throughput of order messages.

**Response:**

The correct answer is **c. Scale out the Kafka broker cluster by adding more brokers during high-volume periods and scale in when the volume decreases.**

### Question 12:
A streaming media company uses Kafka to ingest viewer watch history for real-time recommendation updates. Viewer engagement varies greatly, with peak times during new content releases. To handle variable ingestion rates, which configuration should be optimized?

a. Adjust the replication factor of the watch history topic in real-time to handle the increased data volume.
b. Increase and decrease the number of Kafka Connect Sink Connector tasks to efficiently write watch history data into Kafka.
c. Scale the number of Kafka Streams applications processing the watch history data according to the ingestion rate.
d. Dynamically modify the number of partitions in the watch history topic to manage the load during peak engagement times.

**Response:**

The correct answer is **b. Increase and decrease the number of Kafka Connect Sink Connector tasks to efficiently write watch history data into Kafka.**

### Question 13:
A logistics company tracks shipping containers across the globe in real-time. This tracking information is stored in various formats across different databases. The company aims to centralize this data into Kafka for real-time visibility and to enable reactive logistics management. The IT team is proficient in SQL but new to Kafka. Which approach should they take to integrate this diverse data into Kafka efficiently?

a. Use JDBC Source Connectors to ingest container tracking data into Kafka. Transform this data into a unified format using Kafka Streams for real-time logistics management.
b. Streamline data into Kafka using custom scripts that extract data from databases and publish to Kafka, relying on Kafka Streams for necessary transformations.
c. Consolidate data in the RDBMS using SQL procedures to match the target schema required by Kafka, then use JDBC Source Connectors to stream this unified data into Kafka.
d. Ingest raw data into Kafka using JDBC Source Connectors, then employ ksqlDB to perform SQL-like transformations and route the data to various microservices.

**Response:**

The correct answer is **d. Ingest raw data into Kafka using JDBC Source Connectors, then employ ksqlDB to perform SQL-like transformations and route the data to various microservices.**

### Question 14:
A retail company wishes to analyze customer transactions in real-time to personalize marketing efforts. Transaction data, including purchases and returns, is captured in a legacy system. The marketing team requires this data in a format that can be easily queried and joined with other customer data. Given the team's familiarity with SQL, what is the most effective way to achieve this?

a. Use JDBC Source Connectors to ingest transaction data into Kafka, followed by Kafka Streams for data transformation and enrichment.
b. Directly export transaction data to CSV files, use custom producers to send these files to Kafka, and then apply Kafka Streams for transformation.
c. Ingest transaction data into Kafka using JDBC Source Connectors and leverage ksqlDB for transforming and querying the data in a SQL-like manner.
d. Transform data within the legacy system using SQL stored procedures, then use JDBC Source Connectors to ingest the pre-transformed data into Kafka.

**Response:**

The correct answer is **c. Ingest transaction data into Kafka using JDBC Source Connectors and leverage ksqlDB for transforming and querying the data in a SQL-like manner.**

### Question 15:
An automotive manufacturer aims to optimize its supply chain by analyzing sensor data from its manufacturing equipment in real-time. The sensor data is currently logged in a proprietary format in a traditional database. The operations team, skilled in SQL, seeks to convert this data into actionable insights. Considering their expertise and requirements, which solution would best suit their needs?

a. Utilize JDBC Source Connectors to stream sensor data into Kafka, transforming the data into a more accessible format using Kafka Streams.
b. Export sensor data to a common format like JSON, use Kafka Connect to ingest this data, and then apply ksqlDB to analyze and visualize the data in real-time.
c. Stream sensor data directly into Kafka using custom producers, followed by data transformation through SQL procedures embedded within the Kafka ecosystem.
d. Ingest sensor data into Kafka using JDBC Source Connectors, then use ksqlDB to perform real-time analytics and transform the data for downstream processing.

**Response:**

The correct answer is **d. Ingest sensor data into Kafka using JDBC Source Connectors, then use ksqlDB to perform real-time analytics and transform the data for downstream processing.**

### Question 16:
A company plans to synchronize data between a PostgreSQL database and a Kafka cluster to enable real-time analytics. Which connector should be used to efficiently import data from PostgreSQL into Kafka?

a. JDBC Source Connector
b. S3 Sink Connector
c. Elasticsearch Sink Connector
d. HDFS Sink Connector

**Response:**

The correct answer is **a. JDBC Source Connector.**

**Explanation:**
- **a. JDBC Source Connector**: Correct choice. The JDBC Source Connector is designed to pull data from relational databases like PostgreSQL into Kafka topics, making it suitable for real-time data synchronization and analytics.
- **b. S3 Sink Connector**: Incorrect for this use case. This connector is used to export data from Kafka topics into AWS S3, not for importing data from databases into Kafka.
- **c. Elasticsearch Sink Connector**: Also incorrect for the scenario. While useful for exporting data from Kafka to Elasticsearch for search and analytics, it does not facilitate data import from PostgreSQL to Kafka.
- **d. HDFS Sink Connector**: Not applicable here. This connector exports data from Kafka to HDFS (Hadoop Distributed File System), and does not support importing data from PostgreSQL into Kafka.

### Question 17:
Your organization requires real-time text search capabilities on data streamed through Kafka. Which connector best facilitates exporting data from Kafka topics into Elasticsearch to meet this requirement?

a. JDBC Sink Connector
b. Elasticsearch Sink Connector
c. MongoDB Sink Connector
d. Kafka Connect File Sink Connector

**Response:**

The correct answer is **b. Elasticsearch Sink Connector.**

**Explanation:**
- **a. JDBC Sink Connector**: Incorrect for this requirement. Primarily used for moving data from Kafka to relational databases, not suitable for integration with Elasticsearch for text search.
- **b. Elasticsearch Sink Connector**: Perfect fit. This connector is specifically designed to export data from Kafka topics directly into Elasticsearch, enabling powerful search and analytics capabilities on the streamed data.
- **c. MongoDB Sink Connector**: Incorrect. While MongoDB offers text search capabilities, this connector focuses on exporting data to MongoDB, not Elasticsearch.
- **d. Kafka Connect File Sink Connector**: Incorrect. This connector writes data from Kafka topics to the file system and does not integrate with Elasticsearch or support text search functionalities directly.

### Question 18:
A streaming application is designed to process events in real-time and then store processed events in an Amazon S3 bucket for long-term analysis. Which connector configuration is most appropriate for this use case?

a. JDBC Source Connector
b. S3 Sink Connector
c. MQTT Source Connector
d. File Source Connector

**Response:**

The correct answer is **b. S3 Sink Connector.**

**Explanation:**
- **a. JDBC Source Connector**: Incorrect. This connector is used for importing data from relational databases into Kafka, not for storing data into S3.
- **b. S3 Sink Connector**: Correct. Specifically designed for exporting data from Kafka topics into Amazon S3, this connector supports both the requirement for long-term storage and efficient data analysis.
- **c. MQTT Source Connector**: Incorrect. This connector is utilized for ingesting data from MQTT-enabled devices into Kafka and does not facilitate data export to S3.
- **d. File Source Connector**: Incorrect. It's used for reading data from files into Kafka topics, not for writing or storing data into Amazon S3 from Kafka.

### Question 19:
An IoT company collects temperature and humidity data from sensors deployed in various locations. The goal is to correlate this environmental data with location-specific weather forecasts retrieved from an external API. Which approach best facilitates this integration and processing within the Kafka ecosystem?

a. Ingest sensor data into a Kafka topic using MQTT connectors. Separately, use an external service to fetch weather forecasts, storing this data in a Kafka topic via the HTTP Source Connector. Utilize Kafka Streams to join sensor data with weather forecasts based on location and timestamp, outputting enriched data to another topic.
b. Directly stream sensor data and weather forecasts into Kafka using custom Kafka Producers implemented in Python. These producers perform API calls to retrieve forecasts, merge this data with sensor readings, and produce the combined records into a single Kafka topic.
c. Use the MQTT Source Connector to ingest sensor data into Kafka. Write a Kafka Streams application that performs REST API calls to the weather forecast service for each sensor data record processed, enriching and producing enriched records to a new topic.
d. Implement an MQTT proxy to capture sensor data into Kafka. Concurrently, utilize Kafka Connect with the JDBC Sink Connector to store sensor data in a relational database, from which an external cron job fetches weather forecasts, merges them with sensor data, and re-ingests the enriched data back into Kafka via JDBC Source Connector.

**Response:**

The correct answer is **a. Ingest sensor data into a Kafka topic using MQTT connectors. Separately, use an external service to fetch weather forecasts, storing this data in a Kafka topic via the HTTP Source Connector. Utilize Kafka Streams to join sensor data with weather forecasts based on location and timestamp, outputting enriched data to another topic.**

**Explanation:**
- **a.** Correct. This approach leverages the strengths of Kafka's ecosystem for real-time data integration and processing. MQTT connectors efficiently ingest sensor data, while the HTTP Source Connector handles external API data. Kafka Streams then enables complex processing, such as joining data streams based on key attributes like location and time.
- **b.** While using custom producers allows for flexible data ingestion and preprocessing, it bypasses Kafka's robust and scalable data integration capabilities, such as fault tolerance and parallel processing provided by connectors and Kafka Streams.
- **c.** Integrating external API calls directly within a Kafka Streams application for each record can introduce significant latency and potential rate limit issues with the weather API, making it less efficient for enriching streaming data at scale.
- **d.** This method introduces unnecessary complexity and latency by cycling data through external systems and back into Kafka. It also fails to utilize Kafka's real-time streaming capabilities efficiently, such as stream processing for data enrichment without the need for intermediate storage.

### Question 20:
A retail chain wants to integrate sales data from their Point of Sale (POS) systems across multiple stores into Kafka for real-time analysis and inventory management. Each store's POS system dumps sales records into a local SQL database. The integration needs to account for network bandwidth limitations. Which strategy optimally addresses these requirements?

a. Deploy Kafka Connect with the JDBC Source Connector at each store to ingest sales data into Kafka, using Single Message Transforms (SMTs) to filter and reduce the size of the data on the fly. Aggregate this data centrally using a Kafka Streams application for inventory analysis.
b. Utilize log-based Change Data Capture (CDC) connectors to monitor changes in each store's SQL database, streaming only new or changed sales records into Kafka. This minimizes network usage and enables real-time central processing with Kafka Streams.
c. Implement custom Kafka Producers within the POS systems to directly publish sales data to Kafka, compressing messages to mitigate network bandwidth issues. Use Kafka Streams for processing and inventory management centrally.
d. Set up a central database to aggregate sales data from all stores nightly. Use the JDBC Sink Connector to transfer this aggregated data into Kafka for next-day inventory analysis, relying on batch processing rather than real-time analysis.

**Response:**

The correct answer is **b. Utilize log-based Change Data Capture (CDC) connectors to monitor changes in each store's SQL database, streaming only new or changed sales records into Kafka. This minimizes network usage and enables real-time central processing with Kafka Streams.**

**Explanation:**
- **a.** While JDBC Source Connectors are capable of ingesting data into Kafka, using them without consideration for data volume and network bandwidth can lead to inefficiencies, especially in bandwidth-limited environments.
- **b.** Correct. CDC connectors are designed to efficiently capture and stream only the incremental changes (new or updated sales records), significantly reducing network bandwidth requirements and enabling effective real-time data integration and processing.
- **c.** Custom producers offer flexibility but require additional development and maintenance effort. Compressing messages addresses bandwidth concerns but doesn't reduce the volume of data sent as effectively as streaming only changes.
- **d.** This approach shifts towards batch processing, which does not meet the requirement for real-time analysis and fails to leverage

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
