### Message queue
* A message queue is a durable component, stored in memory, that supports asynchronous communications. It serves as a buffer and distrbutes asynchronous requests. 

* A message queue provides a lightweight buffer which temporarily stores messages, and endpoints that allow software components to connect to the queue in order to send and receive messages.

* To send a message, a component called a producer adds a message to the queue. The message is stored on the queue until another component called a consumer retrieves the message and does something with it.

* Many producers and consumers can use the queue, but each message is processed only once, by a single consumer. For this reason, this messaging pattern is often called one-to-one, or point-to-point, communications. When a message needs to be processed by more than one consumer, message queues can be combined with Pub/Sub messaging in a fanout design pattern.


### Logging, metric, and automation 
Collecting loggs are important when a service scales up. Some metrics that should be considered are:
* Host level metrics: CPU, Memory, Disk I/O etc.
* Business metirc: users, retention, revenue 
* Aggregated level metric: the performance of entire database 

### Database Scaling

* Vertical: in 2013, stackoverflow had over 10 million monthly unique visitors, but it only had 1 master databse.
* Horizontal Scaling: adds more servers alone. 

Some common problems that people might have when it comes to sharding:
* Celebrity Problem: over hitting a popular shard (e.g. John Mayer)
* Resharding data: a single shard can no longer hold more data due to rapid growth/ rehashing required 
* Join and de-normalization: join becomes harder when sharding takes place 

### Summary
* Keep web tier stateless: no sense of session
* Cache data as much as you can 
* Support multiple data center 
* Host static assets in CDN (Content Delivery Network)
* Scale your data tier by sharding
* Split tiers into individual services 
* Monitor using ausomation tools 
