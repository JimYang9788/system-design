### Message queue
* A message queue is a durable component, stored in memory, that supports asynchronous communications. It serves as a buffer and distrbutes asynchronous requests. 

A message queue provides a lightweight buffer which temporarily stores messages, and endpoints that allow software components to connect to the queue in order to send and receive messages.

### Logging, metric, and automation 
Collecting loggs are important when a service scales up. Some metrics that should be considered are:
* Host level metrics: CPU, Memory, Disk I/O etc.
* Business metirc: users, retention, revenue 
* Aggregated level metric: the performance of entire database 
