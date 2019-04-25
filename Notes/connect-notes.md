Producers
Consumers
Brokers
Schema Registry
Stream - Java
KSQL - STREAM


--
Kafka Connect

    File format
    FTP/HTTP/HDFS
    MySQL/PostgreSQL,..

    Data Source
        -- Read from file/mysql/jdbc
        -- produce to kafka topic [producer]

    Data Sink 
        -- subscribe from kafka topic [consumer]
        -- write to file/jdbc/mysql




nano krish-file-source.properties

paste below connect

name=krish-file-source
connector.class=FileStreamSource
tasks.max=1
file=/root/krish/input-file.txt
topic=krish-file-content


Ctrl + 

open second shell on server [ssh again]

kafka-console-consumer --bootstrap-server localhost:9092 --topic krish-file-content --from-beginning



name=krish-file-sink
connector.class=FileStreamSink
tasks.max=1
file=/root/krish/output-file.txt
topics=krish-file-content




CREATE STREAM (topic as input)

PErsisteded query

take data from kafka topics
process the data
output ==> kafka topic



mysql -uroot

CREATE USER 'team'@'%' IDENTIFIED BY 'team1234';

GRANT ALL PRIVILEGES ON *.* TO 'team'@'%' WITH GRANT OPTION;

exit 

mysql -uroot

CREATE DATABASE krish;

USE krish;

create table products (id int, name varchar(255), price int, create_ts timestamp DEFAULT CURRENT_TIMESTAMP , update_ts timestamp DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP );

create table brands (id int, name varchar(255));


{
  "name": "krish-mysql-product-source",
  "config": {
    "connector.class": "io.confluent.connect.jdbc.JdbcSourceConnector",
    "key.converter": "io.confluent.connect.avro.AvroConverter",
    "key.converter.schema.registry.url": "http://k1.nodesense.ai:8081",
    "value.converter": "io.confluent.connect.avro.AvroConverter",
    "value.converter.schema.registry.url": "http://k1.nodesense.ai:8081",
    "connection.url": "jdbc:mysql://localhost:3306/krish?user=team&password=team1234",
    "_comment": "Which table(s) to include",
    "table.whitelist": "products",
    "mode": "timestamp",
     "timestamp.column.name": "update_ts",
    "validate.non.null": "false",
    "_comment": "The Kafka topic will be made up of this prefix, plus the table name  ",
    "topic.prefix": "krish-db-"
  }
}




name=krish-mysql-product-sink
connector.class=io.confluent.connect.jdbc.JdbcSinkConnector
tasks.max=1
topics=brands
connection.url=jdbc:mysql://k1.localhost.ai:3306/krish?user=team&password=team1234
auto.create=true
key.converter=io.confluent.connect.avro.AvroConverter
key.converter.schema.registry.url=http://k1.nodesense.ai:8081
value.converter=io.confluent.connect.avro.AvroConverter
value.converter.schema.registry.url=http://k1.nodesense.ai:8081



kafka-avro-console-producer --broker-list localhost:9092 --topic brands --property value.schema='{"type":"record","name":"brand","fields":[{"name":"id","type":"int"},{"name":"name", "type": "string"}]}'

{"id": 1, "name": "Google"}



