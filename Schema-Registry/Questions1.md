## Question 1

Where does Confluent Schema Registry store the registered schema information?

- A. In Zookeeper under the `/schemas` znode
- B. In a special Kafka topic named `_schemas`
- C. In a relational database configured in Schema Registry
- D. In the Kafka broker's `schema` directory on disk

**Explanation:**
Confluent Schema Registry uses a special Kafka topic named `_schemas` to store the registered schema information. Each schema is stored as a message in this topic, keyed by the schema ID.

- A: While Schema Registry uses Zookeeper for some coordination tasks, schemas themselves are not stored in Zookeeper.
- C: Schema Registry does not use a relational database for schema storage by default. It leverages Kafka for reliable schema storage.
- D: Schemas are not stored on the Kafka broker's disk directly.

**Answer:** B

## Question 2

What serialization formats are supported by the Confluent Schema Registry for storing schemas? (Select two)

- A. Avro
- B. Protobuf
- C. JSON Schema
- D. XML Schema
- E. Thrift

**Explanation:**
The Confluent Schema Registry currently supports two serialization formats for storing schemas:

1. Apache Avro: Avro is a row-based serialization format that is compact, fast, and binary. It's the most commonly used format with the Schema Registry.
2. Protocol Buffers (Protobuf): Protobuf is Google's data interchange format. It's also compact and fast, and it supports schema evolution.

The other options are not currently supported:

- C (JSON Schema) and D (XML Schema) are schema formats for JSON and XML respectively, but they are not supported by the Schema Registry for schema storage.
- E (Thrift) is another serialization format, but it's not currently supported by the Schema Registry.

**Answer:** A, B

## Question 3

Which of the following programming languages have official client libraries for interacting with the Confluent Schema Registry? (Select three)

- A. Java
- B. Python
- C. Go
- D. C++
- E. JavaScript
- F. Ruby

**Explanation:**
The Confluent Schema Registry provides official client libraries for the following programming languages:

1. Java: The Java client is part of the `kafka-schema-registry-client` library.
2. Python: The Python client is provided by the `confluent-kafka` Python package.
3. Go: The Go client is part of the `confluent-kafka-go` package.

The other options do not currently have official client libraries from Confluent:

- D (C++), E (JavaScript), and F (Ruby) can still interact with the Schema Registry using its REST API, but there are no official client libraries provided by Confluent for these languages.

**Answer:** A, B, C

## Question 4

What is the purpose of the compatibility setting in the Confluent Schema Registry?

- A. It defines which serialization format (Avro, Protobuf) is used.
- B. It controls how schemas can evolve over time.
- C. It sets the compatibility between different Schema Registry versions.
- D. It configures compatibility between the Schema Registry and Kafka brokers.

**Explanation:**
The compatibility setting in the Confluent Schema Registry is used to control schema evolution. It defines the rules for how a schema can change over time while still being considered compatible with previous versions.

The available compatibility settings are:

- BACKWARD: A new schema can be used to read data written by an old schema.
- FORWARD: An old schema can be used to read data written by a new schema.
- FULL: Both BACKWARD and FORWARD compatibilities are maintained.
- NONE: No compatibility checks are performed.

The other options are incorrect:

- A is incorrect because the serialization format is not controlled by the compatibility setting.
- C is incorrect because the compatibility setting is about schema versions, not Schema Registry versions.
- D is incorrect because the compatibility setting does not configure compatibility with Kafka brokers.

**Answer:** B

## Question 5

In Avro, what is the effect of adding a field to a record schema without a default value?

- A. It is a backward compatible change
- B. It is a forward compatible change
- C. It is both a backward and forward compatible change
- D. It is an incompatible change

**Explanation:**
In Avro, adding a field to a record schema without a default value is an incompatible change. It breaks both backward and forward compatibility.

- It breaks backward compatibility because data written with the new schema cannot be read by code using the old schema. The old schema will not have a definition for the new field and will not know how to handle it.
- It breaks forward compatibility because data written with the old schema cannot be read by code using the new schema. The new schema will expect the new field to be present, but it will be missing in the old data.

To make adding a field a compatible change, you must provide a default value for the new field. This allows old data to be read by new code (the default is used for the missing field) and new data to be read by old code (the new field is ignored).

Therefore, statements A, B, and C are incorrect. Adding a field without a default is neither a backward nor forward compatible change in Avro.

**Answer:** D

## Question 6

What is the Avro schema evolution rule for removing a field?

- A. It is always a compatible change
- B. It is a backward compatible change
- C. It is a forward compatible change
- D. It is an incompatible change

**Explanation:**
In Avro, removing a field from a record schema is a forward compatible change, but not a backward compatible change.

- It is forward compatible because data written with the old schema can be read by code using the new schema. The new schema simply ignores the removed field when reading old data.
- However, it is not backward compatible because data written with the new schema cannot be read by code using the old schema. The old schema will expect the removed field to be present, but it will be missing in the new data.

Therefore, removing a field allows new code to read old data (forward compatibility), but not old code to read new data (backward compatibility).

Statements A and B are incorrect because removing a field is not always compatible or backward compatible. Statement D is incorrect because removing a field is forward compatible, not completely incompatible.

**Answer:** C

## Question 7

In Avro, what is the compatibility implication of changing the name of a record schema?

- A. It is a backward compatible change
- B. It is a forward compatible change
- C. It is both a backward and forward compatible change
- D. It is an incompatible change

**Explanation:**
In Avro, changing the name of a record schema is an incompatible change. It breaks both backward and forward compatibility.

The name of a record schema is used to identify the schema. When Avro data is serialized, the schema name is included in the serialized data. When the data is deserialized, the deserializer looks for a schema with the same name to use for deserialization.

If the name of a schema is changed:

- Data written with the old schema name cannot be deserialized with the new schema, because the deserializer will not find a schema with the old name. This breaks backward compatibility.
- Data written with the new schema name cannot be deserialized with the old schema, because the deserializer will not find a schema with the new name. This breaks forward compatibility.

Therefore, changing the name of a record schema is an incompatible change. Statements A, B, and C are incorrect.

To evolve a schema while maintaining compatibility, you should not change the name of the schema. Instead, you should evolve the fields within the schema following the Avro compatibility rules.

**Answer:** D

## Question 8

What happens when a Kafka consumer using KafkaAvroDeserializer encounters a message without a schema ID?

- A. The consumer throws a SerializationException
- B. The consumer skips the message and moves to the next one
- C. The consumer attempts to deserialize the message using the latest schema
- D. The consumer falls back to using the GenericRecord deserializer

**Explanation:**
When a Kafka consumer using the KafkaAvroDeserializer encounters a message that does not include a schema ID, it will throw a SerializationException.

The KafkaAvroDeserializer expects messages to be serialized with Confluent Schema Registry and to include the schema ID as part of the message payload. The schema ID is used to retrieve the corresponding schema from the Schema Registry for deserialization.

If a message does not contain a schema ID, the deserializer is unable to determine which schema to use for deserialization, and it cannot proceed. In this case, it will throw a SerializationException to indicate that the message cannot be deserialized due to the missing schema ID.

It's important to ensure that the producer is properly configured to use the KafkaAvroSerializer and that it is registering the schemas with the Schema Registry. This way, the produced messages will include the necessary schema ID for the consumer to deserialize them correctly.

Statement B is incorrect because the consumer does not skip messages without a schema I- D. It throws an exception instead.

Statement C is incorrect because the consumer cannot attempt to deserialize the message using the latest schema if there is no schema ID present. It needs the schema ID to retrieve the correct schema.

Statement D is incorrect because the consumer does not fall back to using a different deserializer when the schema ID is missing. The KafkaAvroDeserializer specifically relies on the schema ID for deserialization.

**Answer:** A

## Question 9

How can you handle a SerializationException thrown by the KafkaAvroDeserializer in a Kafka consumer?

- A. Catch the exception and retry deserializing the message with a different deserializer
- B. Catch the exception and skip the problematic message by committing its offset
- C. Catch the exception and manually retrieve the schema from the Schema Registry for deserialization
- D. Let the exception propagate and handle it at a higher level in the consumer application

**Explanation:**
When a Kafka consumer using the KafkaAvroDeserializer encounters a SerializationException due to a missing schema ID or other deserialization issues, one way to handle it is to catch the exception and skip the problematic message by committing its offset.

Here's how you can approach this:

1. Surround the code that consumes and processes the messages with a try-catch block.
2. In the catch block, if the exception is a SerializationException, log an error message indicating the failed deserialization.
3. Commit the offset of the problematic message using the consumer's commitSync() or commitAsync() method. This tells Kafka that the consumer has processed the message, even though it couldn't deserialize it.
4. Continue consuming the next message.

- B. committing the offset of the problematic message, the consumer acknowledges that it has processed the message and moves on to the next one. This prevents the consumer from getting stuck on the same message indefinitely.

However, it's important to note that skipping messages should be done with caution and only after careful consideration. Skipping messages means losing data, so it's crucial to have proper error handling and monitoring in place to detect and investigate such incidents.

Statement A is incorrect because retrying deserialization with a different deserializer is not a recommended approach. The KafkaAvroDeserializer is specifically designed to work with Confluent Schema Registry and Avro-serialized messages.

Statement C is incorrect because manually retrieving the schema from the Schema Registry is not a practical solution. The deserializer should handle schema retrieval automatically based on the schema ID.

Statement D is partially correct, as letting the exception propagate and handling it at a higher level is another valid approach. However, it doesn't address the specific action of skipping the problematic message by committing its offset.

**Answer:** B

## Question 10

What are the benefits of using Confluent Schema Registry and KafkaAvroDeserializer in a Kafka consumer?

- A. Automatic schema evolution and compatibility checks
- B. Improved deserialization performance compared to generic deserializers
- C. Ability to deserialize messages without knowing the schema upfront
- D. All of the above

**Explanation:**
Using Confluent Schema Registry and KafkaAvroDeserializer in a Kafka consumer offers several benefits:

1. Automatic schema evolution and compatibility checks:
   - The Schema Registry allows you to store and manage schemas for your Kafka messages.
   - It enables schema evolution by allowing you to define compatibility rules for schema changes.
   - The KafkaAvroDeserializer automatically retrieves the appropriate schema from the Schema Registry based on the schema ID included in the message.
   - It ensures that the consumer can deserialize messages even if the schema has evolved, as long as the changes are compatible.

2. Improved deserialization performance compared to generic deserializers:
   - The KafkaAvroDeserializer is optimized for deserializing Avro-serialized messages.
   - It leverages the compact and efficient binary format of Avro, resulting in faster deserialization compared to generic deserializers like JSON.
   - The deserializer also benefits from the schema information stored in the Schema Registry, enabling efficient deserialization without the overhead of including the full schema in each message.

3. Ability to deserialize messages without knowing the schema upfront:
   - When using the KafkaAvroDeserializer, the consumer does not need to have prior knowledge of the schema for the messages it consumes.
   - The deserializer automatically retrieves the schema from the Schema Registry based on the schema ID included in the message.
   - This allows the consumer to deserialize messages from multiple topics or with different schemas without requiring explicit schema management in the consumer code.

- B. leveraging Confluent Schema Registry and KafkaAvroDeserializer, Kafka consumers can benefit from automatic schema evolution, improved deserialization performance, and the ability to deserialize messages without prior knowledge of the schema. These features simplify the development and maintenance of Kafka consumers while ensuring data compatibility and efficiency.

**Answer:** D
