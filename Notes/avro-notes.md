<order>
<no>123232</no>
<amount>10000</amount>
<customer_id>11111</customer_id>
<country>IN</country>
</order> 

Unicode -- x 2 = 220 bytes
    
    JSON
    
{
"no": 123232,
"amount" : 10000,
"customer_id": 11111,
"country" : "IN"
} - x 2 = 142 bytes

Avro Schema - Binary format
[
    [4 bytes number],
    [4 bytes number]
    [4 bytes number]
    [DT [4 bytes], LENGTH [4 bytes], 2 bytes for data [4 bytes] ]

] - 24 bytes
