## Question 21

How can you convert a stream into a table in KSQL?

- A. CREATE TABLE table_name AS SELECT * FROM stream_name;
- B. INSERT INTO table_name SELECT * FROM stream_name;
- C. CREATE TABLE table_name FROM stream_name;
- D. CONVERT STREAM stream_name TO TABLE table_name;

**Explanation:**
The correct syntax to convert a stream into a table in KSQL is `CREATE TABLE table_name AS SELECT * FROM stream_name;`.

- B, C, and D are incorrect because they are not valid syntaxes for this operation in KSQL.

**Answer:** A

## Question 22

What is the purpose of the `AVRO` format in KSQL?

- A. To provide a human-readable format for data
- B. To enable complex data types and schema evolution
- C. To ensure data is stored as plain text
- D. To simplify data parsing

**Explanation:**
The `AVRO` format in KSQL is used to enable complex data types and schema evolution. It is a binary serialization format that supports rich data structures and efficient data encoding.

- A, C, and D are incorrect because they do not accurately describe the purpose and capabilities of the AVRO format.

**Answer:** B

## Question 23

Which KSQL function is used to extract the year from a timestamp?

- A. EXTRACTYEAR()
- B. GETYEAR()
- C. YEAR()
- D. EXTRACT(YEAR FROM timestamp)

**Explanation:**
The `EXTRACT(YEAR FROM timestamp)` function in KSQL is used to extract the year from a timestamp.

- A, B, and C are incorrect because they are not valid KSQL functions for this operation.

**Answer:** D

## Question 24

How do you handle null values in KSQL?

- A. Use the `IS NULL` and `IS NOT NULL` predicates
- B. Use the `NULLIFY()` function
- C. Replace null values with default values using `COALESCE()`
- D. Both A and C

**Explanation:**
In KSQL, you can handle null values using the `IS NULL` and `IS NOT NULL` predicates to filter records, and the `COALESCE()` function to replace null values with default values.

- B is incorrect because `NULLIFY()` is not a valid KSQL function. Combining A and C provides comprehensive handling of null values.

**Answer:** D

## Question 25

Which KSQL function calculates the total sum of a column's values?

- A. SUM()
- B. TOTAL()
- C. ADD()
- D. AGGREGATE()

**Explanation:**
The `SUM()` function in KSQL calculates the total sum of a column's values.

- B, C, and D are incorrect because they are not valid KSQL functions for this operation.

**Answer:** A

## Question 26

What is the default retention period for KSQL streams?

- A. 7 days
- B. 1 day
- C. 1 week
- D. 2 days

### Explanation:

In KSQL, streams are abstractions over Kafka topics. By default, the retention period for Kafka topics—and therefore KSQL streams—is **7 days**. This means that the data in a KSQL stream is retained for 7 days before it is eligible for deletion, unless you configure a different retention period when creating the stream or alter the topic settings.

**Key Points:**

- **Default Retention Period**: 7 days.
- **Inheritance from Kafka**: KSQL streams inherit the retention settings from the underlying Kafka topics.
- **Configuration**: You can adjust the retention period by setting the `RETENTION` property when creating a stream or by modifying the topic configuration directly.

**Example of Setting Retention Period:**

```sql
CREATE STREAM my_stream (
  ...
) WITH (
  kafka_topic='my_topic',
  value_format='JSON',
  retention='168 HOURS'  -- Retention period of 7 days
);
```

**Other Options Explained:**

- **B. 1 day**: Not the default retention period but can be set manually.
- **C. 1 week**: Equivalent to 7 days, but the default is specified in days.
- **D. 2 days**: Not the default retention period but can be configured if needed.


**A. 7 days**

## Question 27

How can you filter records in a KSQL stream?

- A. By using the `FILTER` clause
- B. By using the `WHERE` clause
- C. By using the `HAVING` clause
- D. By using the `LIMIT` clause

**Explanation:**
Records in a KSQL stream can be filtered using the `WHERE` clause, which allows you to specify conditions that records must meet to be included in the query results.

- A, C, and D are incorrect because they are not valid clauses for filtering records in a KSQL stream.

**Answer:** B

## Question 28

Which KSQL function can be used to format timestamps?

- A. FORMAT_TIMESTAMP()
- B. TO_TIMESTAMP()
- C. DATE_FORMAT()
- D. TIMESTAMP_FORMAT()

**Explanation:**
The `DATE_FORMAT()` function in KSQL can be used to format timestamps. It allows you to specify a pattern for formatting the date and time.

- A, B, and D are incorrect because they are not valid KSQL functions for formatting timestamps.

**Answer:** C
