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
A distributed key-value store is also called a distributed hash table, which distributes key-value pair across many servers