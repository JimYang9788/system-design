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
A preset amount of token is put into a user's bucket. Each request consumes a token, we check if there are enough tokens in the bucket. If not, then the request is dropped. The refill is about 4 per 1 minute, (for example)
2. Leaking Bucket 
3. Fixed window counter
4. Sliding window log 
5. Sliding window counter 

