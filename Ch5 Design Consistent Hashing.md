# Design Consistent Hashing 
## The hashing Problem
Suppose you have n cache server, a common way to balance the laod is to use the following hashing method 
```
i = hashkey % N // A modular operation
```
The problem with this hashing algorithm is that if one of the servers fuck up, then the hashing will redistribute, causing the cache to miss, slowing down the entire system. Hence the consistent hashing concept is introduced: 
When a hash table is re-sized and consistent hashig is used, only k/n keys need to be remapped on average, where k is the number of keys, and n is the number of slots. 

Consistent Hashing is a distributed hashing scheme that operates independently of the number of servers or objects in a distributed hash table. It powers many high-traffic dynamic websites and web applications. In this tutorial, Toptal Freelance Software Engineer Juan Pablo Carzolio will walk us through what it is and how hashing, distributed hashing and consistent hashing work.

When a server is removed, only its adjacent neighbour is affected 

### Virtual nodes 
A virtual node refers to the real node, and each server is represented by multiple virtual nodes on the ring. 
Virtual Nodes basically hash a server to mutliple places, so it lives in more places and have the server to be distributed more evenly