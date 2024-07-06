# Kraft vs Zookeeper

Apache Kafka has recently shifted from using Apache ZooKeeper to a new quorum-based controller that uses a consensus protocol called Kafka Raft (KRaft). This change greatly simplifies Kafka's architecture by consolidating responsibility for metadata into Kafka itself, rather than splitting it between ZooKeeper and Kafka\[1]\[3].

The key differences between ZooKeeper mode and KRaft mode in Kafka are:

### Metadata storage

* In ZooKeeper mode, Kafka stores metadata about the controller in ZooKeeper\[3].
* In KRaft mode, metadata is stored in an internal Kafka topic called `__cluster_metadata` which has a single partition\[1]\[3].

### State storage

* KRaft uses an event-sourcing variant of the Raft protocol. State changes are stored as events in the metadata topic, allowing the state to be recreated by replaying the log\[3].
* In ZooKeeper mode, state changes were isolated events without ordering maintained\[3].

### Deployment simplification

* KRaft mode eliminates the need to deploy and manage ZooKeeper alongside Kafka, simplifying administration and monitoring\[3]\[4].
* KRaft also simplifies Kafka startup by removing the need to start multiple daemons required in ZooKeeper mode\[3].

### Scalability

* The number of partitions per cluster was a bottleneck in ZooKeeper mode. KRaft supports a much larger number of partitions\[3].

### Migration

* Migrating an existing ZooKeeper-based cluster to KRaft mode requires a manual multi-phase process to deploy the KRaft controller quorum and migrate brokers\[5].

In summary, KRaft mode greatly simplifies Kafka's architecture, improves scalability, and eliminates the need to deploy and manage ZooKeeper alongside Kafka clusters. However, migrating existing clusters from ZooKeeper to KRaft requires careful planning and execution.

Citations: \[1] https://developer.confluent.io/learn/kraft/ \[2] https://www.baeldung.com/kafka-shift-from-zookeeper-to-kraft \[3] https://redpanda.com/guides/kafka-alternatives/kafka-raft \[4] https://www.linkedin.com/pulse/apache-kafka-study-notes-3-zookeeper-vs-kraft-youssef-ali \[5] https://strimzi.io/blog/2024/03/21/kraft-migration/
