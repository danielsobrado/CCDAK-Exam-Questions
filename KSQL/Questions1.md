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
