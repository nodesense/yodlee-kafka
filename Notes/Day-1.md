
Below Ports to be enabled

Port : 22 [SSH]
Kafka port: 9092 
Zookeeper port: 2181
Rest proxy Port: 8082
Kafka Schema Registry port: 8081
Kafka Connect API: 8083
Kafka control center: 9021
MySQL: 3306
PostgreSQL: 5432 [optional]



create a file called zookeeper.bat, paste below command

zookeeper-server-start %KAFKA_HOME%\etc\kafka\zookeeper.properties

create a file called server-0.bat, paste below command

kafka-server-start %KAFKA_HOME%\etc\kafka\server.properties


create a file called server-1.bat, paste below command

kafka-server-start %KAFKA_HOME%\etc\kafka\server-1.properties


create a file called server-2.bat, paste below command

kafka-server-start %KAFKA_HOME%\etc\kafka\server-2.properties


create a file called server-3.bat, paste below command

kafka-server-start %KAFKA_HOME%\etc\kafka\server-3.properties






zookeeper-server-start %KAFKA_HOME%\etc\kafka\zookeeper.properties

zookeeper-server-start $KAFKA_HOME/etc/kafka/zookeeper.properties


second command prompt

kafka-server-start %KAFKA_HOME%\etc\kafka\server.properties

open third command prompt


kafka-topics --list --zookeeper localhost:2181


kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test


kafka-console-producer --broker-list localhost:9092 --topic test


open forth command prompt

kafka-console-consumer --bootstrap-server localhost:9092 --topic test

kafka-console-consumer --bootstrap-server localhost:9092 --topic test --from-beginning



kafka-console-consumer --bootstrap-server localhost:9092 --topic test --partition 0 --offset 3


kafka-topics --describe --zookeeper localhost:2181 --topic test


kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic messages


kafka-topics --describe --zookeeper localhost:2181 --topic messages


kafka-console-producer --broker-list localhost:9092 --topic messages


kafka-console-consumer --bootstrap-server localhost:9092 --topic messages



kafka-console-consumer --bootstrap-server localhost:9092 --topic messages --from-beginning


kafka-console-consumer --bootstrap-server localhost:9092 --topic messages --partition 0 --offset 0



kafka-console-producer --broker-list localhost:9092 --topic messages --property "parse.key=true" --property "key.separator=:"



kafka-console-consumer --bootstrap-server localhost:9092 --topic messages --from-beginning --property print.key=true --property print.timestamp=true  


 
kafka-console-consumer --bootstrap-server localhost:9092 --topic messages --from-beginning --property print.key=true --property print.timestamp=true   --consumer-property group.id=group1



kafka-topics --create --zookeeper localhost:2181 --replication-factor 3 --partitions 3 --topic logs



kafka-topics --describe --zookeeper localhost:2181 --topic logs


kafka-console-producer --broker-list localhost:9092,localhost:9093,localhost:9094,localhost:9095 --topic logs

kafka-console-consumer --bootstrap-server localhost:9092,localhost:9093,localhost:9094,localhost:9095 --topic logs


Linux/MAC

zookeeper-server-start $KAFKA_HOME/etc/kafka/zookeeper.properties
kafka-server-start $KAFKA_HOME/etc/kafka/server.properties
kafka-server-start $KAFKA_HOME/etc/kafka/server-1.properties
kafka-server-start $KAFKA_HOME/etc/kafka/server-2.properties
kafka-server-start $KAFKA_HOME/etc/kafka/server-3.properties




    <dependencies>
        <dependency>
            <groupId>org.apache.kafka</groupId>
            <artifactId>kafka_2.12</artifactId>
            <version>2.1.1</version>
        </dependency>


        <dependency>
            <groupId>org.apache.kafka</groupId>
            <artifactId>kafka-clients</artifactId>
            <version>2.1.1</version>
        </dependency>
    </dependencies>





