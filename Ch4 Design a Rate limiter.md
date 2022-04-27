# Rate Limiter 
Background: 
A rate limiter is used to control the rate of traffic scent by a client or a service. In the HTTP world, a rate limiter limits the number of clients requests allowed to be sent over a specific period.

Some benefits include:
* Prevent resource starvation caused by DoS attack 
* Reduce cost: Limit costs for some third party (per call API) that the app is using 

Question to ask:
* IP based or User ID Based? (Throttle rules)
* Server side or client side rate limiter?
* Do we need to inform users who are throttled 
* Is it a seperate service or inside of the application code? 


Example wise:
* No more than 2 posts per second 
* Create a maximum of 10 accounts per day from the same IP address 
* Claim a reward 5 times per week 


Implementation:
* Client side is worse than server side: because client is an unreliable place to enforce rules as requests can be forged by malicious actors.
* A middleware between the server and the client side is a good idea
* HTTP 429: too many requests 
* Currently this rate limiter is within a API gateway limiter, other stuff includes (SSL termination, authentification, IP whiitelisting, servicing static content, etc.)
* See if the tech stack has an existing a API gateway for it 

Common Algorithms:
1. Token Bucket (Amazon, Stripe)
A preset amount of token is put into a user's bucket. Each request consumes a token, we check if there are enough tokens in the bucket. If not, then the request is dropped. The refill is about 4 per 1 minute, (for example). Problems with token bucket is its tuning the parameters.

2. Leaking Bucket 
When a request arrives, the system checks if the queue (bucket) is full. If it is not full, the rewuest is dropped. Requests are pulled from the queue and processed at regular intervals. 

Problem is that the queue might be stuffed with old requests. 

3. Fixed window counter
The algorithm divides the timeline into fix-sized time windows and assigne a counter for each window. Each request increments the counter by one. Once the counter reaches the pre-defined threshold, new requests are dropped until a new time window starts. 

* Problem is that, say you limit 5 requests for 1 minute. However, if 5 comes in between 1:30 to 2:00, and 5 come in 2:00 to 2:30, then from 1:30 to 2:30. There's a shit ton requests that you need to work through. (Much more than 5 per minute designed)


4. Sliding window log 
The algorithm keeps track of request timestampe. Time stamp data is usually kept in cache, such as Redis; Remove all old outdated timestamps when new data come in. 
The algorithm will consumes a lot of memoyr because even if a request is rejected, it will still eat the memory away.

5. Sliding window counter 

Mix of fix and sliding window log.

### High level architecture
* Use a in memory cache for fast access (Redis) is an option. It's an in memory store that offers two commands: INCR and EXPIRE:
* INCR increases the stored counter by 1 
* EXPIRE: sets a timeout for the counter. If the timeout expires, the counter is automatically deleted. 
The flow goes:
 
```
┌────────────────┐               ┌────────────────┐              ┌────────────────┐
│                │               │                │              │                │
│                │               │                │              │                │
│    client      ├─────────────► │  Rate Limiter  ├────────────► │    API Server  │
│                │               │                │              │                │
│                │               │                │              │                │
└────────────────┘               └──────┬─────────┘              └────────────────┘
                                        │   ▲
                                        │   │
                                        │   │
                                        │   │
                                        │   │
                                        │   │
                                        ▼   │
                                ┌───────────┴──────┐ 
                                │                  │
                                │    Redis         │
                                │                  │
                                │                  │
                                └──────────────────┘
                                
```

### Deep Dive 
* The rate limiter HTTP requests should return: # of requests remaining, # of rate limit, ratelimit limit and 429 error status code.
* For concurrent execution, using a redis set + lua script is a good way to handle race condition. 
* Use a centralized Redis storage can solve the problem of synchronization (problem of user sends to two different Redis data store)