## Question 1

Which of the following KSQL statements will cause writes to a Kafka topic? (Select two)

- A. `CREATE STREAM FROM_TOPIC AS SELECT * FROM source_topic;`
- B. `CREATE TABLE FROM_TOPIC AS SELECT * FROM source_topic;`
- C. `SELECT * FROM source_topic EMIT CHANGES;`
- D. `DESCRIBE source_topic;`
- E. `SHOW QUERIES;`

**Answer:** A, B

**Explanation:**
In KSQL, `CREATE STREAM AS SELECT` and `CREATE TABLE AS SELECT` statements create new streams or tables based on a query from an existing source. These queries are persistent and continuously write output to a Kafka topic.

- C: `SELECT` statements with `EMIT CHANGES` are transient queries that output to the KSQL console, not to a Kafka topic.
- D, E: `DESCRIBE` and `SHOW QUERIES` are metadata commands that don't write to Kafka topics.

## Question 2

What happens when you run a `CREATE STREAM` statement without an `AS SELECT` clause in KSQL?

- A. It creates a new stream and writes metadata to the KSQL command topic.
- B. It creates a new stream and starts writing data to it from the KSQL application.
- C. It fails because `CREATE STREAM` must always include an `AS SELECT` clause.
- D. It creates a new empty stream but doesn't write anything to Kafka.

**Answer:** A

**Explanation:**
When you run a `CREATE STREAM` statement without an `AS SELECT` clause in KSQL, it registers a new stream on an existing Kafka topic. This metadata is written to the KSQL command topic, but no data is written to the Kafka topic itself.

- B is incorrect because no data is automatically written to the stream from the KSQL application.
- C is incorrect because `AS SELECT` is optional for `CREATE STREAM`. It's required only if you want to create a new stream based on a query from an existing source.
- D is incorrect because while no data is written to Kafka, the metadata is still written to the KSQL command topic.

## Question 3

What is the purpose of the `PARTITIONS` clause in a KSQL `CREATE TABLE` statement?

- A. To specify the number of partitions for the output Kafka topic
- B. To specify the partitioning key for the output Kafka topic
- C. To specify the number of partitions to read from the input Kafka topic
- D. To specify the partitioning key to read from the input Kafka topic

**Answer:** A

**Explanation:**
In a KSQL `CREATE TABLE` statement, the `PARTITIONS` clause is used to specify the number of partitions for the output Kafka topic that will store the table's data.

- B is incorrect because the partitioning key is specified using the `KEY` clause, not `PARTITIONS`.

## Question 4

Which query type is not supported by KSQL?

- A. Stream-to-Stream JOINs
- B. Table-to-Table JOINs
- C. Stream-to-Table JOINs
- D. Complex Nested Queries

**Explanation:**
KSQL supports Stream-to-Stream JOINs, Table-to-Table JOINs, and Stream-to-Table JOINs. However, KSQL does not natively support Complex Nested Queries that require multiple layers of subqueries or highly intricate query structures.

- A, B, and C are incorrect as these are supported query types in KSQL.

**Answer:** D

## Question 5

What is a KSQL table?

- A. A mutable collection of key-value pairs
- B. An immutable, append-only collection of records
- C. A stateful, changelog-based table
- D. A temporary view of streaming data

**Explanation:**
A KSQL table is a stateful, changelog-based table that stores the latest value for each key. It represents a snapshot of the current state based on the changelog of updates.

- A, B, and D are incorrect because they describe different data structures or views in KSQL.

**Answer:** C

## Question 6

Which KSQL function is used to convert a string to uppercase?

- A. UPPER()
- B. TO_UPPER()
- C. STRING_UPPER()
- D. CONVERT_UPPER()

**Explanation:**
The `UPPER()` function in KSQL is used to convert a string to uppercase.

- B, C, and D are incorrect because they are not valid KSQL functions for converting a string to uppercase.

**Answer:** A

## Question 7

What does the `WINDOW` clause in a KSQL query specify?

- A. The time frame for aggregations
- B. The filter condition for the query
- C. The key for partitioning the data
- D. The join condition between streams

**Explanation:**
The `WINDOW` clause in a KSQL query specifies the time frame for aggregations, allowing you to define windows for time-based aggregations such as tumbling, hopping, and session windows.

- B, C, and D are incorrect because they do not define the time frame for aggregations.

**Answer:** A

## Question 8

Which data format is not supported by KSQL for serialization and deserialization?

- A. JSON
- B. Protobuf
- C. Avro
- D. Thrift

**Explanation:**
KSQL supports JSON, Protobuf, and Avro formats for serialization and deserialization. Thrift is not supported by KSQL.

- A, B, and C are incorrect because these formats are supported by KSQL.

**Answer:** D

## Question 9

How can you create a stream in KSQL from an existing Kafka topic?

- A. CREATE STREAM stream_name FROM topic_name;
- B. CREATE STREAM stream_name (columns) WITH (kafka_topic='topic_name', value_format='format');
- C. CREATE STREAM stream_name WITH (kafka_topic='topic_name', value_format='format');
- D. CREATE STREAM stream_name AS SELECT * FROM topic_name;

**Explanation:**
The correct syntax to create a stream in KSQL from an existing Kafka topic is `CREATE STREAM stream_name (columns) WITH (kafka_topic='topic_name', value_format='format');`.

- A is incorrect because it misses the format and column definitions. D is incorrect because it uses a different syntax for creating streams from other streams or tables.

**Answer:** B

## Question 10

What is the purpose of the `PARTITION BY` clause in KSQL?

- A. To split the stream into multiple topics
- B. To repartition the data based on a specified column
- C. To create a new table from a stream
- D. To define the output format of the query

**Explanation:**
The `PARTITION BY` clause in KSQL is used to repartition the data based on a specified column. This is useful for changing the partitioning key of a stream.

- A, C, and D are incorrect because they describe different functionalities in KSQL.

**Answer:** B