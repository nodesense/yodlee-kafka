pre-alloted bytes = new byte[33554432]

[
  { 
 message-1 as bytes,  [4096 bytes]  
 message-2 as bytes, [4096 bytes] 
 message- as bytes,  [4096 bytes]
 message- as bytes,  [4096 bytes]
  } taken as payload, send to broker


 message- as bytes,
 message- as bytes,
 ..
 message-10 as bytes,

]

--

Latency based
        props.put(LINGER_MS_CONFIG, 100); // milli second

[
    {
        {message-1} - 1024 - MS 1,
        {message-2} - 1024 - MS 10,
        {message-3} - 1024 - MS 20,
        ....
        {message-10} - 1024 - MS 100,
    }

]


Latency based
        props.put(LINGER_MS_CONFIG, 100); // milli second

[
    {
        {message-1} - 1024 - MS 0 , size 8192 bytes
        {message-2} - 1024 - MS 10, size 8192 bytes
        {message-3} - 1024 - MS 20,
        ....
        {message-10} - 1024 - MS 100,
    }

]

Acknowledge , to deal with replications

Lead broker accept the message
write to its own commit log -- failure
Leader broker responsible for updating replicas -- failure

Ack - 0 - Lead broker accept the message + 
           Ack immediately 
            [Not written message to commit log]
            Cons: when system crashes before commiting log, we lose message
            Pros: Fast
            
Ack 1 - Lead broker accept the message + 
        write to its own commit log +
        Ack to producer 
        Cons: System failure can affect the message, no replication done yet
        Cons: Slow

Ack All - Lead broker accept the message +
          write to its own commit log +
          Update the replicas, get ack from replicas
          Ack to producer
           Cos: Very Slow
           Pros: Reliablity of messsage


public static String BOOTSTRAP_SERVERS = "k1.example.com:9092,k2.example.com:9092,k3.example.com:9092";

B0 , B1, B2/Lead P0, B3

Producer Ack 0 ==> B1
    Send a message to B1 [should go to B2]
    B1 shall forward to B2 [Lead broker]
    Send a messsage [B2]
    
--

Brokers

Always running [Seed nodes]
B0
B1
B2
B3
B5

---
B6
B7
B8 [ lead for partition 10]

-- 
Consumer 

Topic 
    Partition P0
                [M0, M1, M2, M3.... ... M100] [Write head/end of the log]
                
                Offset: 3
                Commit Offset: {
                      Consumer Group +
                      Topic + Partition 
                }
                
                Commit Offset: 2
                
                Consumer poll, get M0, - failure
                    Commit the offset 0
                    
                Consumer poll, get M1
                    Commit the offset 1

                Consumer got crashed/restarted
                
                 Consumer poll, get M2,
                     Commit the offset 2
                     
                 Consumer poll, get M3,
                     Commit the offset 3
                     
                     
 Commit Offset
 
        Auto - as soon as consumer, receive the messages, 
                auto commit the offset
        Manual
            Sync
            Async