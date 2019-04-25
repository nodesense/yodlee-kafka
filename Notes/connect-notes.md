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
