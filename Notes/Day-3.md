Schema Registry 
    Schema -- template/structure/data-format
    
    Registry the schema in central location
    consumers/producers/streams/connect/rest-proxy can make use of schema registry
    
    REST service/HTTP/GET/POST/DELETE/PUT methods
    Schemas are stored in Zoo Keeper
    
    JSON/XML - TEXT based
    
    <order>
        <no>123232</no>
        <amount>10000</amount>
        <customer_id>11111</customer_id>
        <country>IN</country>
    </order>
    
    JSON
    
    {
       "no": 123232,
       "amount" : 10000,
       "counter_id": 11111,
       "country" : "IN"
    }
    
    
    kafka-topics --zookeeper xyz.nodesense.ai:2181 --create --topic invoices --replication-factor 1 --partitions 3
    
    
    open browser
    
    k1.nodesense.ai:8081
    
    http://k1.nodesense.ai:8081/subjects
    http://k1.nodesense.ai:8081/subjects/invoices-value/versions