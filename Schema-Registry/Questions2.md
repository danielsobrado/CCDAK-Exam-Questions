## Question 11

Which configuration is used to define the schema registry URL in Kafka clients?

- A. `schema.registry.url`
- B. `schema.registry.endpoint`
- C. `schema.registry.address`
- D. `schema.registry.host`

**Explanation:**
The configuration `schema.registry.url` is used to specify the URL of the Confluent Schema Registry in Kafka clients. This URL is necessary for the clients to connect to the Schema Registry and retrieve the schemas.

- Options B, C, and D are incorrect as these are not the correct configuration properties for setting the schema registry URL in Kafka clients.

**Answer:** A

## Question 12

What is the primary purpose of a subject in the Confluent Schema Registry?

- A. To group schemas by type (Avro, Protobuf, JSON Schema)
- B. To manage versions of a schema for a specific topic or entity
- C. To specify the schema storage location
- D. To define the security policies for accessing schemas

**Explanation:**
In the Confluent Schema Registry, a subject is used to manage versions of a schema for a specific topic or entity. It groups schemas together under a common name, and each schema can have multiple versions within that subject.

- Options A, C, and D are incorrect because subjects are specifically for managing schema versions rather than grouping by type, specifying storage locations, or defining security policies.

**Answer:** B

## Question 13

How does Confluent Schema Registry ensure compatibility when registering a new schema version?

- A. By comparing the new schema with the latest version only
- B. By comparing the new schema with all previous versions
- C. By comparing the new schema with a user-specified set of previous versions
- D. By ignoring previous versions and only validating the new schema

**Explanation:**
Confluent Schema Registry ensures compatibility by comparing the new schema with the latest version only. The compatibility check depends on the compatibility mode set for the subject (e.g., BACKWARD, FORWARD, FULL).

- Options B, C, and D are incorrect because the registry typically compares the new schema against the latest version only, not all previous versions or a user-specified set.

**Answer:** A

## Question 14

What compatibility mode allows a new schema to both read data written by an old schema and write data that can be read by the old schema?

- A. BACKWARD
- B. FORWARD
- C. FULL
- D. NONE

**Explanation:**
The FULL compatibility mode allows a new schema to both read data written by an old schema (backward compatibility) and write data that can be read by the old schema (forward compatibility).

- Options A and B are incorrect because BACKWARD only ensures backward compatibility and FORWARD only ensures forward compatibility. Option D (NONE) disables compatibility checks.

**Answer:** C

## Question 15

Which schema type is supported by Confluent Schema Registry but not by Apache Avro?

- A. Protobuf
- B. Thrift
- C. JSON Schema
- D. XML Schema

**Explanation:**
Confluent Schema Registry supports Protobuf and JSON Schema in addition to Avro. Apache Avro natively supports only Avro schemas.

- Options B and D are incorrect as Thrift and XML Schema are not supported by the Schema Registry. Option A is supported by both, but JSON Schema is a type specifically supported by the Schema Registry and not by Apache Avro.

**Answer:** C,A

## Question 16

In the context of Confluent Schema Registry, what is the main advantage of using Avro over JSON Schema?

- A. Avro schemas are human-readable
- B. Avro provides a compact and fast binary serialization format
- C. Avro does not require schema registration
- D. Avro supports all JSON types natively

**Explanation:**
The main advantage of using Avro over JSON Schema is that Avro provides a compact and fast binary serialization format, which is efficient for data storage and transfer.

- Options A and C are incorrect because while Avro schemas can be human-readable, the main advantage is their compactness and speed. Avro does require schema registration. Option D is incorrect as JSON Schema supports JSON types natively, not Avro.

**Answer:** B

## Question 17

What is the default compatibility mode in Confluent Schema Registry?

- A. BACKWARD
- B. FORWARD
- C. FULL
- D. NONE

**Explanation:**
The default compatibility mode in Confluent Schema Registry is BACKWARD. This mode ensures that new schemas can read data produced with earlier versions of the schema.

- Options B, C, and D are incorrect because FORWARD and FULL are not the default modes, and NONE disables compatibility checks.

**Answer:** A

## Question 18

How does the Confluent Schema Registry handle schema evolution?

- A. By automatically converting old schemas to new schemas
- B. By storing all schema versions and applying compatibility checks
- C. By enforcing schema changes directly on the producer side
- D. By modifying the schema directly in the consumer application

**Explanation:**
The Confluent Schema Registry handles schema evolution by storing all schema versions and applying compatibility checks to ensure that new schema versions are compatible with previous versions according to the configured compatibility mode.

- Options A, C, and D are incorrect because the registry does not automatically convert schemas or enforce changes directly on the producer or consumer sides.

**Answer:** B

## Question 19

What happens if a schema fails the compatibility check when being registered in the Confluent Schema Registry?

- A. The schema is registered with a warning
- B. The schema is rejected, and an error is returned
- C. The schema is registered, but compatibility is disabled
- D. The schema is registered with a lower priority

**Explanation:**
If a schema fails the compatibility check when being registered in the Confluent Schema Registry, the schema is rejected, and an error is returned. The schema cannot be registered until it passes the compatibility check.

- Options A, C, and D are incorrect because the registry does not register schemas that fail compatibility checks under any conditions.

**Answer:** B

## Question 20

Which command-line tool can be used to interact with the Confluent Schema Registry?

- A. kafka-schema-registry
- B. schema-registry-cli
- C. confluent-hub
- D. kafka-schema-cli

**Explanation:**
The command-line tool `schema-registry-cli` can be used to interact with the Confluent Schema Registry, allowing users to manage schemas, subjects, and compatibility settings.

- Options A, C, and D are incorrect because they refer to tools that do not interact with the Schema Registry in this manner.

**Answer:** B
