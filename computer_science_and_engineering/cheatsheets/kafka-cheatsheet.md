## Getting Started
1. `cd` to Kafka installation folder
2. Start the Kafka environment
   - first Zookeeper
     ```bash
     $ bin/zookeeper-server-start.sh config/zookeeper.properties
     ```
   - then Kafka
     ```bash
     $ bin/kafka-server-start.sh config/server.properties
     ```
3. Create a topic
   - Create topic
     ```bash
     $ bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
     ```
   - Check topic
     ```bash
     $ bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
     ```
4. Write event to a topic
     ```bash
     $ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
     ```
5. Read from a topic
     ```
     $ $ bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
     ```