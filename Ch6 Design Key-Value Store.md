# Design a key-value store 
### Basics 
A non-relational database must have a unique key, and the value associated with the key can be accessed through the key. For performances reasons, a short key works better. Designing a key value store needs two essential functionalities :

```
put (key,value)
get (key)
```

Some key characteristics:
* The size of a key-value pair is small: less than 10KB 
* Ability to store big data 
* High avaliability + high scalability
* Automatic scaling 
* Tunable consistency * 
* Low latency

### Topic 1 Single Server
Single server version is easy, we can store everythign in a hash table, which keeps everything in memory. To fit more data onto the disk, we can do two optimizations, one is improved compression rate, the other being storing frequently used data in cache.


### Topic 2 Distributed version 
A distributed key-value store is also called a distributed hash table, which distributes key-value pair across many servers. Some key notes to consider are 
1. Consistency: all clients see the same data at the same time 
2. Availability: a client gets the response 
3. Partition Tolerance: a partition is a communication break between two nodes; partition tolerance means the system continues to operate despite network partitions

CAP theorem means you have to sacrifice one to achieve two. Particularly, partition will always occur, so you will have to achieve partition tolerance. We then have to choose between consistency and availability. Why? say we have n1 and n2: if n2 goes down and data comes in, we either accepts the data, then n1 and n2 are out of sync (no consistency); or we don't accept the data (no availability).

### Data Partition
* Use consistent hashing to minimize data movement, this achieves automatic scaling and heterogeneity (the number of virtual nodes for a server is proportional to the server capacity) (server with higher capacity are assigned with more virtual ndoes)

### Data Replication
To achieve high availability and reliability, data must be replicated asynchronously over N servers, where N is a configurable parameter. These N servers are chosen using the following logic: after a key is mapped to a position on the hash ring, walk clockwise from that position and choose the first N on the ring to store the data copies (also to avoid adding & losing data)

### Consistency
* N refers to the total replicas
* R refers to the Read quorum (minimum number of read)
* W refers to the Write quorum (minuum number of write)   
If we have R + W > N, then this is a strong consistency  
If we have R + W < N, then this is a week consistency. 
* Eventual consistency: this is a specific form of weak consistency where given enough time, all updates are propagated, and all replias are consistent. (usually achieved through not accepting new read/write until sall replica agreement). This is not highly available, but is used by Cassandra and Dynamo.


### Hinted handoff
Hinted handoff is a Cassandra feature that optimizes the cluster consistency process and anti-entropy when a replica-owning node is not available, due to network issues or other problems, to accept a replica from a successful write operation.

