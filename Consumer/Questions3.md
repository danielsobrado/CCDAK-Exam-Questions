## Question 21

What is the default value of the `isolation.level` setting in the Kafka consumer configuration?

A. `read_uncommitted`
B. `read_committed`
C. `transactional`
D. `none`

**Explanation:**
The default value of the `isolation.level` setting in the Kafka consumer configuration is `read_uncommitted`. This means that by default, the consumer will read all messages, including transactional messages that are not yet committed. It may consume messages from transactions that are later aborted. If you want the consumer to only read committed messages and wait for transactions to be committed before making the messages visible, you need to explicitly set the `isolation.level` to `read_committed`. The `transactional` and `none` options are not valid values for the `isolation.level` setting.

**Answer:** A

## Question 22

What happens when a consumer with `isolation.level=read_committed` encounters a message that is part of an ongoing transaction?

A. The consumer will read the message immediately
B. The consumer will wait until the transaction is committed before reading the message
C. The consumer will skip the message and move on to the next one
D. The consumer will throw an exception and stop consuming

**Explanation:**
When a consumer with `isolation.level=read_committed` encounters a message that is part of an ongoing transaction, the consumer will wait until the transaction is committed before reading the message. The `read_committed` isolation level ensures that the consumer only reads messages that are not part of ongoing transactions and messages that are part of committed transactions. If a message belongs to a transaction that is still in progress, the consumer will wait until the transaction is committed before making the message visible to the consumer. This behavior guarantees that the consumer only sees messages that are part of successful transactions and prevents the consumer from consuming messages that may later be rolled back if the transaction is aborted.

**Answer:** B