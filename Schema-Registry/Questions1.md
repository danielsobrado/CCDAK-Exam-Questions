## Question 1

Where does Confluent Schema Registry store the registered schema information?

A. In Zookeeper under the `/schemas` znode
B. In a special Kafka topic named `_schemas`
C. In a relational database configured in Schema Registry
D. In the Kafka broker's `schema` directory on disk

**Answer:** B

**Explanation:**
Confluent Schema Registry uses a special Kafka topic named `_schemas` to store the registered schema information. Each schema is stored as a message in this topic, keyed by the schema ID.

- A: While Schema Registry uses Zookeeper for some coordination tasks, schemas themselves are not stored in Zookeeper.
- C: Schema Registry does not use a relational database for schema storage by default. It leverages Kafka for reliable schema storage.
- D: Schemas are not stored on the Kafka broker's disk directly.
- 
## Question 2

What serialization formats are supported by the Confluent Schema Registry for storing schemas? (Select two)

A. Avro
B. Protobuf
C. JSON Schema
D. XML Schema
E. Thrift

**Answer:** A, B

**Explanation:**
The Confluent Schema Registry currently supports two serialization formats for storing schemas:

1. Apache Avro: Avro is a row-based serialization format that is compact, fast, and binary. It's the most commonly used format with the Schema Registry.
2. Protocol Buffers (Protobuf): Protobuf is Google's data interchange format. It's also compact and fast, and it supports schema evolution.

The other options are not currently supported:

- C (JSON Schema) and D (XML Schema) are schema formats for JSON and XML respectively, but they are not supported by the Schema Registry for schema storage.
- E (Thrift) is another serialization format, but it's not currently supported by the Schema Registry.

## Question 3

Which of the following programming languages have official client libraries for interacting with the Confluent Schema Registry? (Select three)

A. Java
B. Python
C. Go
D. C++
E. JavaScript
F. Ruby

**Answer:** A, B, C

**Explanation:**
The Confluent Schema Registry provides official client libraries for the following programming languages:

1. Java: The Java client is part of the `kafka-schema-registry-client` library.
2. Python: The Python client is provided by the `confluent-kafka` Python package.
3. Go: The Go client is part of the `confluent-kafka-go` package.

The other options do not currently have official client libraries from Confluent:

- D (C++), E (JavaScript), and F (Ruby) can still interact with the Schema Registry using its REST API, but there are no official client libraries provided by Confluent for these languages.

## Question 4

What is the purpose of the compatibility setting in the Confluent Schema Registry?

A. It defines which serialization format (Avro, Protobuf) is used.
B. It controls how schemas can evolve over time.
C. It sets the compatibility between different Schema Registry versions.
D. It configures compatibility between the Schema Registry and Kafka brokers.

**Answer:** B

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

## Question 5

In Avro, what is the effect of adding a field to a record schema without a default value?

A. It is a backward compatible change
B. It is a forward compatible change
C. It is both a backward and forward compatible change
D. It is an incompatible change

**Answer:** D

**Explanation:**
In Avro, adding a field to a record schema without a default value is an incompatible change. It breaks both backward and forward compatibility.

- It breaks backward compatibility because data written with the new schema cannot be read by code using the old schema. The old schema will not have a definition for the new field and will not know how to handle it.
- It breaks forward compatibility because data written with the old schema cannot be read by code using the new schema. The new schema will expect the new field to be present, but it will be missing in the old data.

To make adding a field a compatible change, you must provide a default value for the new field. This allows old data to be read by new code (the default is used for the missing field) and new data to be read by old code (the new field is ignored).

Therefore, statements A, B, and C are incorrect. Adding a field without a default is neither a backward nor forward compatible change in Avro.

## Question 6

What is the Avro schema evolution rule for removing a field?

A. It is always a compatible change
B. It is a backward compatible change
C. It is a forward compatible change
D. It is an incompatible change

**Answer:** C

**Explanation:**
In Avro, removing a field from a record schema is a forward compatible change, but not a backward compatible change.

- It is forward compatible because data written with the old schema can be read by code using the new schema. The new schema simply ignores the removed field when reading old data.
- However, it is not backward compatible because data written with the new schema cannot be read by code using the old schema. The old schema will expect the removed field to be present, but it will be missing in the new data.

Therefore, removing a field allows new code to read old data (forward compatibility), but not old code to read new data (backward compatibility).

Statements A and B are incorrect because removing a field is not always compatible or backward compatible. Statement D is incorrect because removing a field is forward compatible, not completely incompatible.

## Question 7

In Avro, what is the compatibility implication of changing the name of a record schema?

A. It is a backward compatible change
B. It is a forward compatible change
C. It is both a backward and forward compatible change
D. It is an incompatible change

**Answer:** D

**Explanation:**
In Avro, changing the name of a record schema is an incompatible change. It breaks both backward and forward compatibility.

The name of a record schema is used to identify the schema. When Avro data is serialized, the schema name is included in the serialized data. When the data is deserialized, the deserializer looks for a schema with the same name to use for deserialization.

If the name of a schema is changed:

- Data written with the old schema name cannot be deserialized with the new schema, because the deserializer will not find a schema with the old name. This breaks backward compatibility.
- Data written with the new schema name cannot be deserialized with the old schema, because the deserializer will not find a schema with the new name. This breaks forward compatibility.

Therefore, changing the name of a record schema is an incompatible change. Statements A, B, and C are incorrect.

To evolve a schema while maintaining compatibility, you should not change the name of the schema. Instead, you should evolve the fields within the schema following the Avro compatibility rules.