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

**Explanation:**
The default retention period for KSQL streams is 1 day. This means data in the stream will be retained for 24 hours before being eligible for deletion.

- A, C, and D are incorrect because they do not reflect the default retention period.

**Answer:** B

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
