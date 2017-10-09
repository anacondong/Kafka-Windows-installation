#Install Kafka for Spring boot Kafka
Reference : http://javasampleapproach.com/java-integration/start-apache-kafka

# download and install 
https://kafka.apache.org/downloads
> copy all properties to your env path


#### start zookeeper server
zookeeper-server-start.bat zookeeper.properties
#### start Kafka server
kafka-server-start.bat server.properties
#### Kafka Topic
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic jsa-test
#### Kafka Producer to send messages
kafka-console-producer.bat --broker-list localhost:9092 --topic jsa-test
#### Start a Kafka consumer
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic jsa-test --from-beginning


#### Create a multi-broker cluster
# Create a config file for new brokers >>> Create file properties for start up multi server
####
server1.properties:
    broker.id=1
    listeners=PLAINTEXT://:9093
    log.dirs=kafka-logs-1
 ####
server2.properties:
    broker.id=2
    listeners=PLAINTEXT://:9094
    log.dirs=kafka-logs-2


#### Start new nodes
kafka-server-start.bat server-1.properties

#### Create a new topic for replication
kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 3 --partitions 1 --topic jsa-replicated-topic
#### Check the replicated-topic by describe topics command:
kafka-topics.bat --describe --zookeeper localhost:2181 --topic jsa-replicated-topic

#### Create a Producer to replicated-topic
kafka-console-producer.bat --broker-list localhost:9092 --topic jsa-replicated-topic
#### Consumer to replicated-topic
kafka-console-consumer.bat --bootstrap-server localhost:9092 --from-beginning --topic jsa-replicated-topic


#### Fault-Tolerance
wmic process get processid,caption,commandline | find "java.exe" | find "server1.properties"

taskkill /pid 422 /f

