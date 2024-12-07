## Question 21

What is the purpose of the Confluent Schema Registry in a Kafka ecosystem?

- A. To store and manage Avro schemas for Kafka messages
- B. To provide a REST API for producing and consuming Kafka messages
- C. To handle authentication and authorization for Kafka clients
- D. To monitor and manage Kafka clusters and their performance

**Explanation:**
The primary purpose of the Confluent Schema Registry in a Kafka ecosystem is to store and manage Avro schemas for Kafka messages. The Schema Registry provides a centralized repository for storing and retrieving Avro schemas, enabling schema evolution and compatibility checks. It allows Kafka producers and consumers to work with structured data in Avro format, ensuring that the data adheres to a well-defined schema. By storing schemas in the Schema Registry, producers and consumers can refer to schemas by their unique identifier, eliminating the need to include the full schema with each message. This promotes schema reuse, reduces message size, and enables schema evolution over time.

**Answer:** A

## Question 22

How does the Confluent Schema Registry ensure compatibility between different versions of a schema?

- A. By enforcing strict backward compatibility for all schema changes
- B. By allowing schema changes that are both backward and forward compatible
- C. By automatically generating compatibility reports for schema versions
- D. By using a compatibility setting to define allowed schema evolution rules

**Explanation:**
The Confluent Schema Registry uses a compatibility setting to define allowed schema evolution rules and ensure compatibility between different versions of a schema. The compatibility setting determines how schema changes are validated and whether they are allowed or rejected. The available compatibility settings are:

- BACKWARD: A new schema is backward compatible with the latest version.
- FORWARD: A new schema is forward compatible with the latest version.
- FULL: A new schema is both backward and forward compatible with the latest version.
- NONE: No compatibility checks are performed.

- B. configuring the appropriate compatibility setting, you can control the schema evolution process and ensure that new schema versions are compatible with existing data and applications. The Schema Registry validates schema changes against the configured compatibility rules and rejects changes that violate the rules. This helps maintain data integrity and prevents incompatible schema changes from being registered.

**Answer:** D

## Question 23

How can you retrieve the latest version of a schema from the Confluent Schema Registry using its REST API?

- A. Send a GET request to `/subjects/<subject>/versions/latest`
- B. Send a POST request to `/schemas/<subject>/versions/latest`
- C. Send a GET request to `/schemas/<subject>/versions/latest`
- D. Send a POST request to `/subjects/<subject>/versions/latest`

**Explanation:**
To retrieve the latest version of a schema from the Confluent Schema Registry using its REST API, you need to send a GET request to the endpoint `/subjects/<subject>/versions/latest`. The `<subject>` placeholder represents the name of the schema subject for which you want to retrieve the latest version. The Schema Registry organizes schemas into subjects, typically corresponding to Kafka topic names. By making a GET request to this endpoint, the Schema Registry will respond with the latest version of the schema associated with the specified subject. The response will include the schema details, such as the schema ID, version number, and the schema definition itself.

**Answer:** A

## Question 24

What is the purpose of the `kafkastore.topic` configuration in the Confluent Schema Registry?

- A. To specify the Kafka topic where the Schema Registry stores its schema data
- B. To define the compatibility setting for schema evolution
- C. To set the frequency at which the Schema Registry checks for schema updates
- D. To configure the retention period for old schema versions

**Explanation:**
The `kafkastore.topic` configuration in the Confluent Schema Registry is used to specify the Kafka topic where the Schema Registry stores its schema data. The Schema Registry uses Kafka as its underlying storage mechanism to persist and distribute schema information across multiple instances. By default, the Schema Registry creates a Kafka topic named `_schemas` to store the schema data. However, you can customize the topic name by setting the `kafkastore.topic` configuration property. This allows you to use a different topic name if needed, such as in cases where you have multiple Schema Registry instances or want to segregate schema data from other Kafka topics.

**Answer:** A

## Question 25

What is the default compatibility setting in the Confluent Schema Registry for schema evolution?

- A. BACKWARD
- B. FORWARD
- C. FULL
- D. NONE

**Explanation:**
The default compatibility setting in the Confluent Schema Registry for schema evolution is BACKWARD. When a new version of a schema is registered, the Schema Registry checks its compatibility with the existing schema versions based on the configured compatibility setting. The BACKWARD compatibility setting ensures that a new schema can be used to read data written by the previous schema version. In other words, consumers using the new schema can still read data produced with the old schema. This is the most commonly used compatibility setting as it allows for schema evolution while maintaining backward compatibility. If you require different compatibility rules, you can change the setting to FORWARD, FULL, or NONE based on your specific requirements.

**Answer:** A

## Question 26

How can you change the compatibility setting for a specific subject in the Confluent Schema Registry using its REST API?

- A. Send a PUT request to `/config/<subject>`
- B. Send a POST request to `/config/<subject>`
- C. Send a PUT request to `/compatibility/<subject>`
- D. Send a POST request to `/compatibility/<subject>`

**Explanation:**
To change the compatibility setting for a specific subject in the Confluent Schema Registry using its REST API, you need to send a PUT request to the endpoint `/config/<subject>`. The `<subject>` placeholder represents the name of the schema subject for which you want to modify the compatibility setting. In the request body, you need to provide the new compatibility setting as a JSON object. For example:

```json
{
  "compatibility": "FULL"
}
```

## Question 27

What is the impact of removing a required field that has a default value in an Avro schema?

- A. It is a backward compatible change
- B. It is a forward compatible change
- C. It is both a backward and forward compatible change
- D. It is an incompatible change

**Explanation:**
Removing a required field that has a default value in an Avro schema is a backward compatible change. It means that data written with the new schema can be read by applications using the old schema.

When a field with a default value is removed, the old schema still expects that field to be present. However, when reading data written with the new schema (which doesn't contain the removed field), the old schema will automatically fill in the default value for the missing field. This ensures that the old schema can still read and process the data correctly.

On the other hand, removing a required field is not a forward compatible change. Applications using the new schema will not be able to read data written with the old schema because the required field is expected but not present in the old data.

Therefore, removing a required field with a default value is a backward compatible change, allowing old applications to read data written with the new schema, but not vice versa.

**Answer:** A

## Question 28

What compatibility level is maintained when adding a new optional field to an Avro schema?

- A. Backward compatibility
- B. Forward compatibility
- C. Full compatibility
- D. No compatibility

**Explanation:**
Adding a new optional field to an Avro schema maintains both backward and forward compatibility, which is known as full compatibility.

When a new optional field is added to the schema, it means that the field is not required and has a default value. This change is backward compatible because data written with the new schema can be read by applications using the old schema. The old schema will simply ignore the additional optional field that it doesn't recognize.

Additionally, adding an optional field is forward compatible because data written with the old schema can still be read by applications using the new schema. The new schema will treat the missing optional field as having its default value.

Since both backward and forward compatibility are maintained, adding a new optional field to an Avro schema provides full compatibility. It allows both old and new applications to read data written with either schema version.

**Answer:** C

## Question 29

What is the effect of changing the data type of a field in an Avro schema?

- A. It is a backward compatible change
- B. It is a forward compatible change
- C. It is both a backward and forward compatible change
- D. It is an incompatible change

**Explanation:**
Changing the data type of a field in an Avro schema is an incompatible change. It breaks both backward and forward compatibility.

When the data type of a field is changed, it means that the serialized representation of the data for that field is different between the old and new schemas. Applications using the old schema will not be able to deserialize data written with the new schema correctly because the data type of the field has changed. Similarly, applications using the new schema will not be able to deserialize data written with the old schema correctly.

For example, if a field's data type is changed from an integer to a string, the serialized data will be different, and the applications expecting an integer will fail to deserialize the string value correctly.

Therefore, changing the data type of a field in an Avro schema is an incompatible change that breaks both backward and forward compatibility. It requires careful consideration and coordination between producers and consumers to handle the schema evolution properly.

**Answer:** D

## Question 30

In the Confluent Schema Registry, what is the default compatibility setting for new schemas?

- A. BACKWARD
- B. FORWARD
- C. FULL
- D. NONE

**Answer:** A

**Explanation:**
The Confluent Schema Registry default compatibility type is BACKWARD . The main reason that BACKWARD compatibility mode is the default, and preferred for Kafka, is so that you can rewind consumers to the beginning of the topic.

- BACKWARD compatibility means that data written with a new schema can be read by code using an old schema.
- FORWARD compatibility means that data written with an old schema can be read by code using a new schema.
- FULL compatibility means that both BACKWARD and FORWARD compatibilities are required.
- NONE means that no compatibility checking is performed.
