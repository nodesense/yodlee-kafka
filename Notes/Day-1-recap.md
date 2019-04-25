Broker - Roles & Responsiblities
    Manages the partitions/data
    Accept messages from producers, 
    write to commit log/file system/append only file system
        kafka-logs
            <<topic-name>>-<<partition-id>>
                    00000000000000000000.log [all messages are stored]
                    00000000000000000000.index [offset to message map]    
                    00000000000000000000.timeindex [timestamp to offset]
                    
    serve the consumer
        - consumer pull the messages from broker 
                topic + partition 

Producer
    -- data source /messages/event/ data is generated here
    -- push the messages to broker
         topic
         partition index

Consumer 
    -- poll & pull the data
          
Cluster
    -- group of brokers
    -- zookeeper 
    -- broker id
    -- ip address: port number
    -- storage location

Topic
    Contract between consumer/producer
    Producer publish the messages into topic
    Consumer subscribe the messages from topic

    Partition
        Subset of topic messages
        Offset

        ordered delivery - single partition 
        Message

        Replication

Leader is a broker, but leader for a partition
    Takes all the write calls
    Responsible for updating replicas

Zookeeper
    Topic management
    Broker connections
