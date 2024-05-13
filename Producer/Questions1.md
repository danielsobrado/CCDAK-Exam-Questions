## Question 1

What happens when you set `max.in.flight.requests.per.connection` to a value greater than 1 in a Kafka producer?

A. It increases the throughput of the producer
B. It increases the latency of the producer
C. It can lead to out-of-order delivery of messages
D. It has no effect on the producer's behavior

**Answer:** C

**Explanation:**
Setting `max.in.flight.requests.per.connection` to a value greater than 1 in a Kafka producer allows multiple requests to be sent to the broker in parallel, without waiting for the previous requests to be acknowledged. While this can improve throughput, it also means that if a request fails and needs to be retried, the subsequent requests may have already been processed, leading to out-of-order delivery.

- A is incorrect because while it can improve throughput, it's not the only effect.
- B is incorrect. In fact, it can potentially decrease latency by allowing more requests in flight.
- D is incorrect because it does have a significant effect on the producer's behavior.

## Question 2

What is the effect of setting `acks=0` in a Kafka producer?

A. The producer will wait for the broker to acknowledge the message before sending the next one
B. The producer will wait for the leader and all replicas to acknowledge the message
C. The producer will not wait for any acknowledgement from the broker
D. The producer will throw an exception if the broker does not acknowledge the message

**Answer:** C

**Explanation:**
When `acks` is set to 0 in a Kafka producer, the producer will not wait for any acknowledgement from the broker before considering the send operation successful. This means the producer will fire and forget the message, providing no guarantees about whether the broker has received it.

- A and B are incorrect because they describe the behaviors of `acks=1` and `acks=all` respectively.
- D is incorrect because no exception is thrown in this case. The producer simply continues sending messages without waiting for acknowledgement.

## Question 3

What is the relationship between `request.timeout.ms` and `delivery.timeout.ms` in a Kafka producer?

A. `request.timeout.ms` should always be greater than `delivery.timeout.ms`
B. `delivery.timeout.ms` should always be greater than `request.timeout.ms`
C. They should always be set to the same value
D. They are independent and can be set to any value

**Answer:** B

**Explanation:**
In a Kafka producer, `request.timeout.ms` configures the maximum amount of time the client will wait for a response from the server when sending a request, while `delivery.timeout.ms` sets an upper bound on the time to report success or failure to the application after a call to `send()`.

If `delivery.timeout.ms` is smaller than `request.timeout.ms`, the client can time out and report a failure to the application before the request timeout even occurs. Therefore, `delivery.timeout.ms` should always be greater than `request.timeout.ms` to allow the full request timeout to elapse before reporting a timeout failure to the application.

- A is incorrect because it's the other way around.
- C is incorrect because they serve different purposes and can have different values.
- D is incorrect because there is a recommended relationship between the two settings.

## Question 4

A Kafka producer application needs to send messages to a topic. The messages do not require any particular order. Which of the following properties are mandatory in the producer configuration? (Select two)

A. `compression.type`
B. `partitioner.class`
C. `bootstrap.servers`
D. `key.serializer`
E. `value.serializer`
F. `client.id`

**Answer:** C, E

**Explanation:**
For a Kafka producer application to function, it must know how to connect to the Kafka cluster and how to serialize the message values. Therefore, the mandatory properties are:

- `bootstrap.servers`: This specifies the list of Kafka brokers the producer should contact to bootstrap initial cluster metadata.
- `value.serializer`: This specifies the serializer class for message values.

The other options are not strictly mandatory:

- A: `compression.type` is optional. If not set, the producer will send uncompressed messages.
- B: `partitioner.class` is optional. If not set, the default partitioner will be used, which is sufficient for messages that don't require a particular order.
- D: `key.serializer` is only required if the messages have keys. It's not mandatory if the messages don't have keys.
- F: `client.id` is optional. It's used to identify the producer application, but the producer will work without it.
