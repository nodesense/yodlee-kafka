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
