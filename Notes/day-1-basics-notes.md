# Single partition

Topic:Messages 

Producer send/put messages
    producer decides which partition the message should be stored
    M0 (key/value), M1, M2, M3
    Key is optional/null
    Key is used for deciding partition

    When key is null,
        Round robin [P0, P1, P2, P0, P1, P2, P0]
    When key is not null, keys is byte[]
        Hash Code from byte[] % MAX PARTITIONS

        HC - 324343 % 2 = either 0, 1
        HC - 324343 % 3 = either 0, 1, 2
Broker
 Messages
    Partition 0
        [M0, M1, M2, M3]

Consumer
    Read Data/Poll/Pull from topic & partition
      Read data M0, M1, M2, M3

# Multiple partitions
Topic:Messages 
Producer send messages
    M0 [Key/Value], M1, M2, M3
Broker
 Messages
    Partition 0
        [M0, M2] -- offset for partition 0
    Partition 1
        [M1, M3]  -- offset for partition `
Consumer
    Read Data/Poll/Pull from topic & partition(s)
      Read data From P0
      Read data From P1

      [M0, M1, M3, M2]

---
Partitions [P0, P1, P2, P3]

Consumer Group - group1

 Single   Consumer 1 -  [P0, P1, P2]

 Two consumers - group1
 ===========
 Consumer 1 -  [P0, P1]
 Consumer 2 -  [P2]

 Three consumers - group1
 ===========
 Consumer 1 -  [P0]
 Consumer 2 -  [P1]
 Consumer 3 -  [P2]


 Four consumers
 ===========
 Consumer 1 -  [P0]
 Consumer 2 -  [P2]
 Consumer 3 -  [P1]
 Consumer 4 -  []


 6 consumers
 ===========
 Consumer 1 -  [P0]
 Consumer 2 -  [P2]
 Consumer 3 -  []- idle
 Consumer 4 -  [] - idle
 Consumer 5 -  [P1]
 Consumer 6 -  [] - idle


Product connect to Broker localhost:9095

Product get meta data for topic logs

