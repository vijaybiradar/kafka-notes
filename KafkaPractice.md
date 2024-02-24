**#Question:**

Your supermarket company has a Kafka cluster consisting of three brokers. They intend to utilize Kafka to track purchases for inventory management purposes. As a part of this setup, they require you to create a topic named inventory_purchases with specific configurations. The topic should have 6 partitions and a replication factor of 3. Additionally, you need to publish test data to this topic and verify its consumption. Utilizing Kafka's command line tools, provide step-by-step instructions to accomplish these tasks. Ensure the instructions are clear and detailed.

**Solution:**

**Creating the Kafka Topic:**

Create the Kafka topic inventory_purchases with the required configurations using the kafka-topics command:

````
kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 3 --partitions 6 --topic inventory_purchases
```
Note: If a warning appears suggesting avoiding topic creation with a '.', ignore it as it's likely referring to a different naming convention.

**Publishing Test Data to the Topic:**

Start a command line producer to publish test data to the inventory_purchases topic:

```
kafka-console-producer --broker-list localhost:9092 --topic inventory_purchases
Enter the test data in the producer console. For example:

>product: apples, quantity: 5
>product: lemons, quantity: 7
```
Once you've entered the test data, you can exit the producer.

**Verifying Data Consumption:**

Start a command line consumer to consume data from the inventory_purchases topic:

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic inventory_purchases --from-beginning
```
Note: The --from-beginning flag ensures that all existing messages in the topic are consumed.

You should see the test messages you published earlier:


product: apples, quantity: 5
product: lemons, quantity: 7
This confirms that the test data was successfully published to the topic inventory_purchases and consumed by the consumer.




**#Question:**

Your team is tasked with configuring Kafka topics to customize their behavior. Specifically, you need to enable unclean.leader.election for the inventory_purchases topic and set a 3-day retention period for both the cluster default and the existing inventory_purchases topic. Using Kafka command-line tools, provide step-by-step instructions to achieve these objectives.

**Answer:**

Enabling unclean.leader.election for the inventory_purchases Topic:

Use the kafka-configs command to set unclean.leader.election.enable to true for the inventory_purchases topic:

```
kafka-configs --zookeeper localhost:2181 --entity-type topics --entity-name inventory_purchases --alter --add-config unclean.leader.election.enable=true
```
Setting a 3-Day Retention Period for the Cluster Default and the Existing inventory_purchases Topic:

Set the default retention period for the cluster to 259200000 milliseconds (3 days) using the kafka-configs command with entity-default:

```
kafka-configs --bootstrap-server localhost:9092 --entity-type brokers --entity-default --alter --add-config log.retention.ms=259200000
```
Set the retention period for the inventory_purchases topic to 259200000 milliseconds (3 days) using the kafka-configs command:

```
kafka-configs --zookeeper localhost:2181 --entity-type topics --entity-name inventory_purchases --alter --add-config retention.ms=259200000
```
These steps will enable unclean.leader.election for the inventory_purchases topic and set a 3-day retention period for both the cluster default and the existing inventory_purchases topic.





**Question:**

You're tasked with setting up Kafka consumers to consume data from a specific topic using multiple consumer groups. One consumer group will have a single consumer, while the other will have two consumers. Your goal is to observe how different consumer groups handle messages in Kafka. Provide step-by-step instructions to achieve the following objectives:

Set Up the First Consumer Group with One Consumer:

Set up the first consumer as the sole consumer in its group. Consume messages from the inventory_purchases topic and save the output to /home/cloud_user/output/group1_consumer1.txt.

Set Up a Second Consumer Group with Two Consumers:

Create a consumer in a separate group, and store its output in /home/cloud_user/output/group2_consumer1.txt.
Create a second consumer in the same group, and store its output in /home/cloud_user/output/group2_consumer2.txt.
Ensure that the consumers are consuming data from the inventory_purchases topic.

**Answer:**

Set Up the First Consumer Group with One Consumer:

Use the following command to set up the first consumer as the sole consumer in its group and save the output to /home/cloud_user/output/group1_consumer1.txt:

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic inventory_purchases --group 1 > /home/cloud_user/output/group1_consumer1.txt
```
Set Up a Second Consumer Group with Two Consumers:

Create a consumer in a separate group, and store its output in /home/cloud_user/output/group2_consumer1.txt:

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic inventory_purchases --group 2 > /home/cloud_user/output/group2_consumer1.txt
```
Create a second consumer in the same group, and store its output in /home/cloud_user/output/group2_consumer2.txt:

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic inventory_purchases --group 2 > /home/cloud_user/output/group2_consumer2.txt
````
By following these steps, you'll have set up two consumer groups with different configurations for consuming data from the inventory_purchases topic.






**Question:**

You're tasked with setting up and running a Kafka Streams application to process real-time data. Follow the provided objectives to achieve this task.

Objectives:

Create the Input and Output Topic:
Create the input topic named streams-plaintext-input.

```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic streams-plaintext-input
```
Create the output topic named streams-wordcount-output.

```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic streams-wordcount-output
```
Open a Kafka Console Producer:
Open a Kafka console producer to input data.

```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic streams-plaintext-input
```
Input the following three messages:

markdown
Copy code
> kafka streams is great
> kafka processes messages in real time
> kafka helps real information streams
Open a Kafka Console Consumer:
Open a Kafka console consumer with specific properties.

```
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
--topic streams-wordcount-output \
--from-beginning \
--formatter kafka.tools.DefaultMessageFormatter \
--property print.key=true \
--property print.value=true \
--property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
--property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```
Run the Kafka Streams Application:
Use the kafka-run-class.sh command to run the WordCountDemo application.

```
bin/kafka-run-class.sh org.apache.kafka.streams.examples.wordcount.WordCountDemo
```
**Answer:**

Create the Input and Output Topic:

Input Topic Creation:
```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic streams-plaintext-input
```
Output Topic Creation:
```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic streams-wordcount-output
```
Open a Kafka Console Producer:
Open a Kafka console producer to input data.

```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic streams-plaintext-input
```
Input the following three messages:

markdown
Copy code
> kafka streams is great
> kafka processes messages in real time
> kafka helps real information streams
Open a Kafka Console Consumer:
Open a Kafka console consumer with specific properties.

```
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 \
--topic streams-wordcount-output \
--from-beginning \
--formatter kafka.tools.DefaultMessageFormatter \
--property print.key=true \
--property print.value=true \
--property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer \
--property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
```
Run the Kafka Streams Application:
Use the kafka-run-class.sh command to run the WordCountDemo application.

```
bin/kafka-run-class.sh org.apache.kafka.streams.examples.wordcount.WordCountDemo
```
By following these steps, you'll set up the input and output topics, input some sample data, open a consumer to view the processed output, and run the Kafka Streams application to perform real-time data processing.





**Question:**

You're tasked with implementing a basic Kafka Connect connector to export data from the inventory_purchases topic to a file on the local disk. Follow the provided objectives to achieve this task.

**Objectives:**

Create a Connector to Export Data from the inventory_purchases Topic to a File:
Create a FileStreamSinkConnector to export data from the inventory_purchases topic to a file located at /home/cloud_user/output/output.txt.

Explain:
Use the provided curl command to create the connector named file_sink. The command specifies the configuration including the connector class, maximum number of tasks, output file path, topic to consume data from, and converter classes for key and value.

```
curl -X POST http://localhost:8083/connectors -H "Content-Type: application/json" -d '{
  "name": "file_sink",
  "config": {
    "connector.class": "org.apache.kafka.connect.file.FileStreamSinkConnector",
    "tasks.max": "1",
    "file": "/home/cloud_user/output/output.txt",
    "topics": "inventory_purchases",
    "key.converter": "org.apache.kafka.connect.storage.StringConverter",
    "value.converter": "org.apache.kafka.connect.storage.StringConverter"
  }
}'
```
Verify Data Export:

Start a console producer to publish data representing a purchase of plums to the inventory_purchases topic:
```
kafka-console-producer --broker-list localhost:9092 --topic inventory_purchases
```
Publish the data: plums:5.
Check the file /home/cloud_user/output/output.txt to verify that the purchase of plums shows up in the file data:
```
cat /home/cloud_user/output/output.txt
```
Note: It may take a few moments for the connector to process the new data.

Answer:

Create a Connector to Export Data from the inventory_purchases Topic to a File:
Execute the provided curl command to create the connector named file_sink:

```
curl -X POST http://localhost:8083/connectors -H "Content-Type: application/json" -d '{
  "name": "file_sink",
  "config": {
    "connector.class": "org.apache.kafka.connect.file.FileStreamSinkConnector",
    "tasks.max": "1",
    "file": "/home/cloud_user/output/output.txt",
    "topics": "inventory_purchases",
    "key.converter": "org.apache.kafka.connect.storage.StringConverter",
    "value.converter": "org.apache.kafka.connect.storage.StringConverter"
  }
}'
```
Verify Data Export:

Start a console producer to publish data representing a purchase of plums to the inventory_purchases topic:
```
kafka-console-producer --broker-list localhost:9092 --topic inventory_purchases
```
Publish the data: plums:5.
Check the file /home/cloud_user/output/output.txt to verify that the purchase of plums shows up in the file data:
```
cat /home/cloud_user/output/output.txt
```
Note: It may take a few moments for the connector to process the new data.





**Question:**

Confluent provides a REST Proxy interface that serves as a RESTful interface on top of your Kafka cluster. In this lab, you'll work with Confluent REST Proxy to publish messages to Kafka topics. Follow the objectives to complete the task.

Objectives:

Publish the Missing Records to the inventory_purchases Topic:
Use an HTTP request to Confluent REST Proxy to publish the missing records to the inventory_purchases topic. The records to publish are:

Key: "apples", Value: "23"
Key: "grapes", Value: "160"
Explain:
Use the provided curl command to send an HTTP POST request to Confluent REST Proxy. This request includes JSON-formatted records with keys and values for publishing to the inventory_purchases topic.

```
curl -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \
  -H "Accept: application/vnd.kafka.v2+json" \
  --data '{"records":[{"key":"apples","value":"23"},{"key":"grapes","value":"160"}]}' \
  "http://localhost:8082/topics/inventory_purchases"
```
Verify that the data is in the topic using a console consumer:

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic inventory_purchases --from-beginning --property print.key=true
```
Publish the Missing Records to the member_signups Topic:
Use an HTTP request to Confluent REST Proxy to publish the missing records to the member_signups topic. The records to publish are:

Key: "77543", Value: "Rosenberg, Willow"
Key: "56878", Value: "Giles, Rupert"
Explain:
Use the provided curl command to send an HTTP POST request to Confluent REST Proxy. This request includes JSON-formatted records with keys and values for publishing to the member_signups topic.

```
curl -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \
  -H "Accept: application/vnd.kafka.v2+json" \
  --data '{"records":[{"key":"77543","value":"Rosenberg, Willow"},{"key":"56878","value":"Giles, Rupert"}]}' \
  "http://localhost:8082/topics/member_signups"
```
Verify that the data is in the topic using a console consumer:

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic member_signups --from-beginning --property print.key=true
```
Answer:

Publish the Missing Records to the inventory_purchases Topic:
Use the provided curl command to publish the missing records to the inventory_purchases topic:

```
curl -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \
  -H "Accept: application/vnd.kafka.v2+json" \
  --data '{"records":[{"key":"apples","value":"23"},{"key":"grapes","value":"160"}]}' \
  "http://localhost:8082/topics/inventory_purchases"
```
Verify that the data is in the topic using a console consumer:

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic inventory_purchases --from-beginning --property print.key=true
```
Publish the Missing Records to the member_signups Topic:
Use the provided curl command to publish the missing records to the member_signups topic:

```
curl -X POST -H "Content-Type: application/vnd.kafka.json.v2+json" \
  -H "Accept: application/vnd.kafka.v2+json" \
  --data '{"records":[{"key":"77543","value":"Rosenberg, Willow"},{"key":"56878","value":"Giles, Rupert"}]}' \
  "http://localhost:8082/topics/member_signups"
```
Verify that the data is in the topic using a console consumer:

```
kafka-console-consumer --bootstrap-server localhost:9092 --topic member_signups --from-beginning --property print.key=true

```
