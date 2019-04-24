# Kafka connect

KAFKA CONNECT


    File format
    FTP/HTTP/HDFS
    MySQL/PostgreSQL,..

    Data Source
        -- Read from file/mysql/jdbc
        -- produce to kafka topic [producer]

    Data Sink 
        -- subscribe from kafka topic [consumer]
        -- write to file/jdbc/mysql
        

confluent status connectors

mkdir krish

cd krish

touch krish-file-source.properties
touch krish-file-sink.properties

touch input-file.txt
touch output-file.txt



nano krish-file-source.properties

paste below connect

name=krish-file-source
connector.class=FileStreamSource
tasks.max=1
file=/root/krish/input-file.txt
topic=krish-file-content



Ctrl + O to save the file
Ctrl + X to exit

cat krish-file-source.properties


confluent status connectors

# LOAD the CONNECTORS

confluent load krish-file-source -d krish-file-source.properties

confluent status connectors




# open second shell on server [ssh again]

kafka-console-consumer --bootstrap-server localhost:9092 --topic krish-file-content --from-beginning




#on shell 1, on krish directory

echo "line 1" >> input-file.txt


# for unloading connectors

confluent unload krish-file-source 


# krish-file-sink.properties

nano krish-file-sink.properties


name=krish-file-sink
connector.class=FileStreamSink
tasks.max=1
file=/root/krish/output-file.txt
topics=krish-file-content


Ctrl + W [Enter]
Ctrl + X


# To load connector

confluent load krish-file-sink -d krish-file-sink.properties

confluent status connectors
confluent status krish-file-sink


# MYSQL


mysql -uroot

CREATE USER 'team'@'%' IDENTIFIED BY 'team1234';

GRANT ALL PRIVILEGES ON *.* TO 'team'@'%' WITH GRANT OPTION;

exit 

## For ALL

mysql -uroot

CREATE DATABASE krish;

USE krish;

create table products (id int, name varchar(255), price int, create_ts timestamp DEFAULT CURRENT_TIMESTAMP , update_ts timestamp DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP );



# open second shell (ssh)

cd krish

touch krish-mysql-product-source.json

nano krish-mysql-product-source.json

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


# Load connector

confluent load krish-mysql-product-source -d krish-mysql-product-source.json



kafka-avro-console-consumer --bootstrap-server localhost:9092 --topic krish-db-products --from-beginning


insert into products (id, name, price) values(10,'moto phone 10', 1000); 

insert into products (id, name, price) values(20,'moto phone 20', 2000); 


update products set price=2200 where id=20;


# Sink
Sink

touch krish-mysql-product-sink.properties

nano krish-mysql-product-sink.properties


name=krish-mysql-product-sink
connector.class=io.confluent.connect.jdbc.JdbcSinkConnector
tasks.max=1
topics=krish-brands
connection.url=jdbc:mysql://k1.localhost.ai:3306/krish?user=team&password=team1234
auto.create=true
key.converter=io.confluent.connect.avro.AvroConverter
key.converter.schema.registry.url=http://k1.nodesense.ai:8081
value.converter=io.confluent.connect.avro.AvroConverter
value.converter.schema.registry.url=http://k1.nodesense.ai:8081


confluent load krish-mysql-product-sink -d krish-mysql-product-sink.properties


create table brands (id int, name varchar(255));


kafka-avro-console-producer --broker-list localhost:9092 --topic brands --property value.schema='{"type":"record","name":"brand","fields":[{"name":"id","type":"int"},{"name":"name", "type": "string"}]}'


paste below without new line

{"id": 1, "name": "Google"}
