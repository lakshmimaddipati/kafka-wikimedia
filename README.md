# kafka-wikimedia
  kafka producer consumer in java
![image](https://github.com/user-attachments/assets/818e5627-e4d2-424e-8104-d2bf2faa57b1)

##### kafka-wikimedia
- Recent Change Stream: https://codepen.io/Krinkle/pen/BwEKgW?editors=1010
- EventSource Analytics: https://esjewett.github.io/wm-eventsource-demo/

#### Overview:
- We get data from wikimedia as a stream through a kafka producer in to kafka topics. We then create a kafka consumer that takes this data and send to opensearch for analytics. Kafka is started locally using docker with [Conduktor](https://conduktor.io/) console for the GUI. Apache Zookeeper is used for service synchronization and naming registry. 
- The messages are compressed using snappy for high throughput at producer level.
- When records are being sent to kafka topics, in the case where no partition and no key is specified, the default partitioner partitions records in a round-robin fashion. While this spreads records out evenly among the partitions, it also results in more batches that are smaller in size. That is why I implemented sticky partitioner. By “sticking” to a partition until the batch is full (or is sent when linger.ms is up), we can create larger batches and reduce latency in the system compared to the default partitioner. After sending a batch, the partition that is sticky changes. Over time, the records should be spread out evenly among all the partitions.
- [Opensearch](https://opensearch.org/) is a fork of ElasticSerach and Kibana. Locally it is setup using docker as well. The messages are delivered in atleast once fashion and offsets are commited as soon as they are processed. To prevent duplicate processing of messages, idempotency is maintained.

- Once the project is setup and docker containers are running, conduktor ui can be found on ```localhost:8080``` and opensearch dashboard can be found on ```localhost:5601```

#### Conduktor UI:
![image](https://github.com/user-attachments/assets/59d71f33-6d92-44d2-abc3-5cb1708307e5)
![WhatsApp Image 2024-09-02 at 12 19 28_a115ac6a](https://github.com/user-attachments/assets/100376dc-7212-4445-bcc1-b88a269ecbf6)
![WhatsApp Image 2024-09-02 at 12 19 57_9fdfa910](https://github.com/user-attachments/assets/198a0e22-5663-470a-b3e3-4d67d1789521)

#### OpenSearch Dashboards:
![image](https://github.com/user-attachments/assets/a4b5bc31-f5c5-46d4-a6c8-bcbf517e5f71)

