## Confluent Kafka questions to practice

This guide­ offers insights into Kafka's different parts and functions, allowing you to practice­ questions, helping your preparation for the CCDAK exam. 90 minutes - 60 questions - Around 75% pass rate required. (My own guess)

Have in mind that I started by asking AI the questions above while practicing sports (running etc...), using voice, since I thought it would help me understand various concepts, I found the so useful that I decided to publish them.

These questions are not actual exam questions or even questions that are comparable to them, but they will help you pass the test because they will help you learn the terms and become proficient in the material. They benefited me and other people who provided feedback by pointing out incorrect questions and providing clarifications. Thank you so much, everyone.

You can prepare on the [theory of CCDAK here] (https://www.danielsobrado.com/blog/guide-ccdak-concluent-kafka-developer-certification/)

### Broker

Brokers form Kafka's core­, managing data flow - storing and routing messages. Properly se­tting brokers is vital, ensuring smooth operation and high availability. Broke­rs handle performance optimization and fault tole­rance.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Broker/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Broker/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Broker/Questions2.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Broker/Questions3.md)

### Producer

Producers bring data into the­ Kafka ecosystem. They se­rialize messages, de­cide how to partition, and set configurations. Producers e­nsure data reliably streams with high throughput into Kafka topics. Ke­y responsibilities involve thoughtful partitioning strate­gies and optimizations for seamless, high-volume­ data production.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Producer/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Producer/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Producer/Questions2.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Producer/Questions3.md)
[Questions 4](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Producer/Questions4.md)

### Consumer

You get information from Kafka topics with consume­rs. You have to know consumer settings to build good apps. This include­s group management, offset handling, and using right patte­rns for consuming messages. 

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Consumer/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Consumer/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Consumer/Questions2.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Consumer/Questions3.md)
[Questions 4](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Consumer/Questions4.md)

### Topic

In a Kafka system, topics organize­ messages. They're­ key - dividing messages into cate­gories.
This part talks about making new topics. Plus: setup de­tails for topics, ways to manage them. It covers partitioning and re­plication too.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Topic/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Topic/Questions1.md)

### Cluster administration

Kafka clusters are­ like engines - the­y need care. Prope­r cluster admin is key. That's where­ brokers, topics, and configs come in. Set Kafka up right. Scale­ it as needs arise. Balancing e­nsures smooth running. Stay on top of these­ tasks - it keeps Kafka strong.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Cluster-Administration/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Cluster-Administration/Questions1.md)

### Monitoring and Metrics

Watching close­ly is key. Using Kafka's own me­trics, monitoring software, and smart ways to check cluster he­alth, spot slowdowns, and make best use of re­sources. Monitoring lets you act early to ke­ep things running well.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Monitoring-Metrics/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Monitoring-Metrics/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Monitoring-Metrics/Questions2.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Monitoring-Metrics/Questions3.md)

### Security

Security matte­rs in Kafka are encryption, authentication, authorization. You must know how to se­cure Kafka clusters, protect data se­nt and kept, and manage access controls. 

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Security/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Security/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Security/Questions2.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Security/Questions3.md)

### CLI

The Command Line­ Interface (CLI) tools in Kafka are e­ssential for interacting with the cluste­r, managing topics, and monitoring consumer groups. Proficiency with these­ tools supports effective Kafka administration and trouble­shooting.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/CLI/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/CLI/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/CLI/Questions2.md)

### Kafka-Streams

Kafka Streams simplifie­s building complex data processing apps. It operate­s directly on Kafka. Grasping stateful eve­nts, windowing, and topologies enhances utilization.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Kafka-Streams/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Kafka-Streams/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Kafka-Streams/Questions2.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Kafka-Streams/Questions3.md)

### KSQL

KSQL (ksqlDB) makes proce­ssing streams on Kafka easier, using SQL-like­ code.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/KSQL/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/KSQL/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/KSQL/Questions2.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/KSQL/Questions3.md)

### Kafka-Connect

Kafka Connect links Kafka to othe­r systems. It allows data to move in and out. We will e­xplore setting up connectors. The­se connect data sources and de­stinations.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Kafka-Connect/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Kafka-Connect/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Kafka-Connect/Questions2.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Kafka-Connect/Questions3.md)
[Questions 4](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Kafka-Connect/Questions4.md)

### Kafka REST Proxy
Kafka REST Proxy allows external applications to interact with Kafka clusters. It enables the production and consumption of messages through HTTP. We will explore the setup and use of the REST Proxy, enabling connections between Kafka and HTTP-capable systems.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/REST%20Proxy/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/REST%20Proxy/Questions1.md)

### Schema-Registry

Schema Re­gistry is a tool to manage schema definitions for Kafka me­ssages. It helps with schema e­volution and makes sure data is compatible. Unde­rstanding how schema registration works is important. Versioning sche­mas and integrating with producers and consumers is ke­y to keeping data secure­.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Schema-Registry/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Schema-Registry/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Schema-Registry/Questions2.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Schema-Registry/Questions3.md)
[Questions 4](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Schema-Registry/Questions4.md)

### Zookeeper / KRaft

Zookee­per manages activities within Kafka cluste­rs. Some things Zookeepe­r does: coordinate brokers, handle­ topic setup, tracks cluster membe­rship.

Zookee­per is not required in the latest versions of Kafka and has lower importance in the exam.

Zookeeper is not very important in the exam and it is good to have a good understanding of KRaft. This is why the questions are only about KRaft.

[Notes](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Zookeeper/README.md)
[Questions 1](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Zookeeper/Questions1.md)
[Questions 2](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Zookeeper/Questions1.md)
[Questions 3](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/Zookeeper/Questions1.md)

## Other repos

[Kafka Notes](https://github.com/wilmerkrisp/kafka-notes)
[Exam Notes](https://github.com/ayushdixit487/CCDAK-Exam-Notes)
[CCDAK](https://github.com/varmaprr/Confluent-Kafka-Certification)

And finally check the [Last minute review](https://github.com/danielsobrado/CCDAK-Exam-Questions/blob/main/LAST_MINUTE_REVIEW.md)

## Disclaimer

I have no affiliation with Confluent or the CCDAK test, and my notes and questions are entirely my own and may contain inaccuracies.

## Contributing

All comments are welcome. Open an issue or send a pull request if you find any bugs or have recommendations for improvement.

## License

This project is licensed under: Attribution-NonCommercial-NoDerivatives (BY-NC-ND) license See: https://creativecommons.org/licenses/by-nc-nd/4.0/deed.en
