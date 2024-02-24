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
