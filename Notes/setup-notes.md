JDK 1.8 
IntelliJ/Eclipse
Confluent Kafka Community Edition


javac -version

confluent-2.x.zip file
extract the file

move the confluent-5.2.1 into c:\
c:\confluent-5.2.1

Environment Variables
JAVA_HOME c:\program files\Java\jdk1.8_XYZ
KAFKA_HOME c:\confluent-5.2.1

two command prompts

in command prompt,

zookeeper-server-start %KAFKA_HOME%\etc\kafka\zookeeper.properties

second command prompt

kafka-server-start %KAFKA_HOME%\etc\kafka\server.properties



Distributed Computing - Introduction
KAFKA - Introduction
