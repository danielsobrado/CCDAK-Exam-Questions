## Question 21

Which security protocol in Kafka provides both encryption and authentication for client-broker communication?

- A. PLAINTEXT
- B. SASL_PLAINTEXT
- C. SSL
- D. SASL_SSL

**Explanation:**
The SASL_SSL security protocol in Kafka provides both encryption and authentication for client-broker communication. SASL_SSL combines the benefits of SSL/TLS encryption with SASL authentication mechanisms. When SASL_SSL is used, the data exchanged between clients and brokers is encrypted using SSL/TLS, ensuring confidentiality and integrity. Additionally, SASL authentication is employed to authenticate the clients and brokers. SASL (Simple Authentication and Security Layer) supports various authentication mechanisms, such as PLAIN, SCRAM, and GSSAPI (Kerberos). By using SASL_SSL, you can achieve both encryption and authentication in a single protocol, providing a high level of security for your Kafka cluster.

**Answer:** D
