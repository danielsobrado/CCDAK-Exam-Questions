## Question 11

Which KSQL function is used to concatenate two strings?

- A. CONCAT()
- B. JOIN()
- C. MERGE()
- D. APPEND()

**Explanation:**
The `CONCAT()` function in KSQL is used to concatenate two strings.

- B, C, and D are incorrect because they are not valid KSQL functions for concatenating strings.

**Answer:** A

## Question 12

What is the role of the `KEY` keyword in KSQL table creation?

- A. To define the primary key of the table
- B. To specify the partitioning key of the table
- C. To assign a unique identifier to each record
- D. To create an index on the table

**Explanation:**
The `KEY` keyword in KSQL table creation is used to specify the primary key of the table, which is also used for partitioning.

- B, C, and D are incorrect because they do not accurately describe the role of the `KEY` keyword in KSQL.

**Answer:** A

## Question 13

Which statement is true about KSQL streams?

- A. They store historical data indefinitely
- B. They are append-only collections of immutable records
- C. They can be directly queried for the current state
- D. They do not support windowed aggregations

**Explanation:**
KSQL streams are append-only collections of immutable records that represent the continuous flow of data.

- A, C, and D are incorrect because streams do not store data indefinitely, they represent immutable records, and they do support windowed aggregations.

**Answer:** B

## Question 14

Which KSQL command is used to terminate a running query?

- A. DROP QUERY
- B. STOP QUERY
- C. TERMINATE
- D. DELETE QUERY

**Explanation:**
The `TERMINATE` command is used to stop a running query in KSQL.

- A, B, and D are incorrect because they are not valid commands for terminating a query in KSQL.

**Answer:** C

## Question 15

What is the result of executing the following KSQL query: `SELECT * FROM my_stream EMIT CHANGES;`?

- A. It creates a new table from the stream
- B. It continuously outputs the current state of the stream
- C. It returns a snapshot of the stream at a point in time
- D. It filters records based on a condition

**Explanation:**
The `SELECT * FROM my_stream EMIT CHANGES;` query continuously outputs the current state of the stream, providing a real-time view of the data as it arrives.

- A, C, and D are incorrect because they do not describe the behavior of the `EMIT CHANGES` clause.

**Answer:** B

## Question 16

Which clause in KSQL is used to define the duration of a hopping window?

- A. SIZE
- B. DURATION
- C. HOP
- D. WINDOW

**Explanation:**
The `SIZE` clause is used to define the duration of a hopping window in KSQL.

- B, C, and D are incorrect because they are not valid clauses for defining the duration of a hopping window in KSQL.

**Answer:** A

## Question 17

How can you perform an inner join between two streams in KSQL?

A. `CREATE STREAM new_stream AS SELECT * FROM stream1 INNER JOIN stream2 WITHIN 5 MINUTES ON stream1.key = stream2.key;`

B. `CREATE STREAM new_stream AS SELECT * FROM stream1 JOIN stream2 ON stream1.key = stream2.key;`

C. `CREATE STREAM new_stream AS SELECT * FROM stream1 LEFT JOIN stream2 WITHIN 5 MINUTES ON stream1.key = stream2.key;`

D. `CREATE STREAM new_stream AS SELECT * FROM stream1 CROSS JOIN stream2 ON stream1.key = stream2.key;`

---

**Explanation:**

The correct syntax to perform an inner join between two streams in KSQL is:

**Option A:**

```sql
CREATE STREAM new_stream AS
SELECT *
FROM stream1
INNER JOIN stream2
WITHIN 5 MINUTES
ON stream1.key = stream2.key;
```

- **`INNER JOIN`**: Specifies that an inner join is to be performed between `stream1` and `stream2`.
- **`WITHIN 5 MINUTES`**: Defines a time window of 5 minutes for the join. This is mandatory for stream-to-stream joins in KSQL to handle the temporal nature of streaming data.
- **`ON stream1.key = stream2.key`**: The join condition based on matching keys.

**Option B** is incorrect because it lacks the `WITHIN` clause, which is required when performing stream-to-stream joins in KSQL. Without the `WITHIN` clause, the join operation cannot properly align the streaming data over time.

**Option C** is incorrect because it uses a `LEFT JOIN`, which performs a **left outer join**, not an inner join. This means it would include all records from `stream1` and the matching records from `stream2`, which is not the same as an inner join.

**Option D** is incorrect because `CROSS JOIN` is not supported between streams in KSQL. Additionally, even if it were, a cross join produces the Cartesian product of the two streams, which is not an inner join.

**Answer:**

**A.** `CREATE STREAM new_stream AS SELECT * FROM stream1 INNER JOIN stream2 WITHIN 5 MINUTES ON stream1.key = stream2.key;`

## Question 18

What does the `GROUP BY` clause do in a KSQL query?

- A. It filters records based on a condition
- B. It partitions the data by a specified key
- C. It aggregates data based on specified columns
- D. It orders the data by a specified column

**Explanation:**
The `GROUP BY` clause in a KSQL query aggregates data based on specified columns, allowing for calculations like COUNT, SUM, AVG, etc., over grouped records.

- A, B, and D are incorrect because they describe different functionalities in KSQL.

**Answer:** C

## Question 19

Which keyword is used to create a persistent query in KSQL?

- A. PERSIST
- B. CREATE STREAM AS
- C. CREATE PERSISTENT QUERY
- D. SAVE

**Explanation:**
The `CREATE STREAM AS` keyword is used to create a persistent query in KSQL. This query continuously processes the data and stores the results in a new stream.

- A, C, and D are incorrect because they are not valid keywords for creating a persistent query in KSQL.

**Answer:** B

## Question 20

Which function is used to calculate the number of records in a KSQL stream?

- A. COUNT()
- B. SUM()
- C. AVG()
- D. MAX()

**Explanation:**
The `COUNT()` function is used to calculate the number of records in a KSQL stream.

- B, C, and D are incorrect because they perform different types of calculations.

**Answer:** A
