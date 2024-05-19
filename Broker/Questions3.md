## Question 21

Can Kafka's zero-copy optimization be used in combination with compression?

- A. Yes, zero-copy and compression can be used together seamlessly.
- B. No, zero-copy is incompatible with compression and cannot be used together.
- C. Zero-copy can be used with compression, but it requires additional configuration.
- D. Zero-copy is automatically disabled when compression is enabled.

**Answer:** A

**Explanation:**
Kafka's zero-copy optimization can be used in combination with compression seamlessly. Zero-copy and compression are independent features that can work together to optimize data transfer and storage in Kafka.

Here's how zero-copy and compression can be used together:

1. Producer-side compression:
   - Before sending data to Kafka, the producer application can compress the data using a compression algorithm supported by Kafka, such as Gzip, Snappy, or LZ4.
   - Compression reduces the size of the data, which can help save network bandwidth and storage space.

2. Zero-copy data transfer:
   - When the producer sends the compressed data to Kafka, Kafka uses zero-copy optimization to transfer the compressed data directly from the file system cache to the network buffer.
   - Zero-copy operates on the compressed data without any modifications or decompression.

3. Broker-side storage:
   - Kafka brokers store the compressed data as-is, without decompressing it.
   - Storing compressed data helps optimize storage utilization and reduces the storage footprint of the Kafka cluster.

4. Consumer-side decompression:
   - When the consumer receives the compressed data from Kafka, it needs to decompress the data before processing it.
   - The consumer is responsible for decompressing the data using the same compression algorithm used by the producer.

Zero-copy and compression can work together seamlessly because zero-copy operates on the compressed data without any modifications. It transfers the compressed data efficiently from the producer to the consumer, while compression helps reduce the data size and optimize storage.

Using zero-copy with compression does not require any additional configuration (option C) and is not automatically disabled when compression is enabled (option D). Kafka supports the combination of zero-copy and compression out of the box.

- B. leveraging both zero-copy and compression, Kafka can achieve efficient data transfer, reduced network bandwidth usage, and optimized storage utilization, leading to improved overall performance and scalability of the Kafka cluster.

## Question 22

What is the relationship between the `replication.factor` of a topic and the `min.insync.replicas` setting?

- A. `min.insync.replicas` must be less than or equal to the `replication.factor`
- B. `min.insync.replicas` must be greater than the `replication.factor`
- C. `min.insync.replicas` and `replication.factor` are independent settings
- D. `min.insync.replicas` must be equal to the `replication.factor`

**Explanation:**
The `replication.factor` of a topic and the `min.insync.replicas` setting are related, and there is a specific requirement for their values. The `min.insync.replicas` setting specifies the minimum number of in-sync replicas that must acknowledge a write for the write to be considered successful. For the producer to successfully write messages to a topic, the number of in-sync replicas must be greater than or equal to the `min.insync.replicas` value. Therefore, `min.insync.replicas` must be less than or equal to the `replication.factor` of the topic. If `min.insync.replicas` is set higher than the `replication.factor`, writes to the topic will fail because there won't be enough in-sync replicas to satisfy the `min.insync.replicas` requirement.

**Answer:** A

## Question 23

What happens when a producer sends a message with `acks=all` to a topic that has a `min.insync.replicas` value greater than the number of currently in-sync replicas?

- A. The producer will receive an acknowledgment and the write will succeed
- B. The producer will receive an error indicating that the `min.insync.replicas` requirement is not met
- C. The producer will wait indefinitely until the number of in-sync replicas meets the `min.insync.replicas` requirement
- D. The producer will ignore the `min.insync.replicas` setting and write the message successfully

**Explanation:**
When a producer sends a message with `acks=all` to a topic that has a `min.insync.replicas` value greater than the number of currently in-sync replicas, the producer will receive an error indicating that the `min.insync.replicas` requirement is not met. The write operation will fail because the number of in-sync replicas is insufficient to satisfy the durability requirement specified by `min.insync.replicas`. The producer will not wait indefinitely for the number of in-sync replicas to increase, nor will it ignore the `min.insync.replicas` setting. Instead, it will immediately return an error to the producer, indicating that the write could not be completed successfully due to the lack of enough in-sync replicas.

**Answer:** B

## Question 24

What happens if you set both `log.retention.ms` and `log.retention.minutes` configurations in Kafka?

- A. The larger value will take precedence
- B. The smaller value will take precedence
- C. Kafka will use an average of both values
- D. It will result in a configuration error

**Explanation:**
When you set both `log.retention.ms` and `log.retention.minutes` configurations in Kafka, the smaller value will take precedence. Kafka uses the smallest value among the retention configurations to determine the retention period for log segments. If `log.retention.ms` is set to a smaller value than `log.retention.minutes` (after converting minutes to milliseconds), Kafka will use the value specified in `log.retention.ms` as the retention period. Similarly, if `log.retention.minutes` is set to a smaller value, Kafka will use that value. This allows for flexibility in specifying the retention period using different units of time, while ensuring that the most restrictive value is applied.

**Answer:** B

## Question 25

How can you set a retention period of 2 weeks for a specific topic in Kafka?

- A. Set `retention.ms=1209600000` in the topic configuration
- B. Set `log.retention.hours=336` in the broker configuration
- C. Set `log.retention.ms=1209600000` in the broker configuration
- D. Set `retention.ms=1209600000` in the broker configuration

**Explanation:**
To set a retention period of 2 weeks for a specific topic in Kafka, you need to set the `retention.ms` parameter in the topic configuration. The `retention.ms` parameter specifies the retention period in milliseconds. To calculate the value for 2 weeks, you can use the following formula:

2 weeks = 14 days
1 day = 24 hours
1 hour = 60 minutes
1 minute = 60 seconds
1 second = 1000 milliseconds
2 weeks = 14 days * 24 hours/day * 60 minutes/hour * 60 seconds/minute * 1000 milliseconds/second
= 1,209,600,000 milliseconds

Therefore, setting `retention.ms=1209600000` in the topic configuration will configure a retention period of 2 weeks for that specific topic. Setting the retention period in the broker configuration using `log.retention.hours` or `log.retention.ms` would apply the retention period to all topics in the cluster, not just a specific topic.

**Answer:** A

## Question 26

What is the default value of the `log.retention.hours` configuration in Kafka?

- A. 168 hours (1 week)
- B. 24 hours (1 day)
- C. 720 hours (30 days)
- D. Infinite retention

**Explanation:**
The default value of the `log.retention.hours` configuration in Kafka is 168 hours, which is equivalent to 1 week. If no retention period is explicitly set using `log.retention.ms`, `log.retention.minutes`, or `log.retention.hours`, Kafka will retain log segments for a period of 1 week by default. This means that log segments older than 1 week will be automatically deleted by Kafka to free up storage space. However, it's important to note that the actual retention period can be influenced by other factors, such as the `log.retention.bytes` configuration, which limits the total size of log segments retained, and the `log.segment.bytes` configuration, which determines the size of individual log segments.

**Answer:** A