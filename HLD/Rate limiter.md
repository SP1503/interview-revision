### DDOS: Distributed Denial of service
## Overview of system
- System used to allow only x number of requests from a user within y timeframe.
- This helps to restrict the total number of requests a user can make within the given timeframe.
- The rest of the requests are being restrict with response code as 429 says too many requests.
- Helps in preventing
	- DDOS 
	- servers gets overwhelmed by the requests.
## MVP Functional Requirements:
- Limit based on some criteria
- Limits and criteria should be configurable
	- No of request:
	- Criteria
		- UserId
		- Ip Address
		- Session Id
	- Time frame:
		- Per second
		- Per minute
		- Per Day
- Allow or block request based on the configured criteria
## TradeOff - Non Functional Requirements:
- **CAP or PACELC**
	- **Consistency vs Availability**:
		- Partition tolerance will be there in distributed system
			- What is meaning of consistency here?
				- Number of request per time frame
			- What is meaning of availability here?
				- Is the rate limiter always needs to be available.
		- [Availability is more important] compared to consistency. Okay with eventual consistency.
		- Consistency ability is 1000 req per second but configure 95-req per second.
	- **Consistency vs Latency**:
		- As we are going with [eventual consistency], the [latency will be always low]

## Scale Estimation:
- Daily users of twitter: 3 * 10 ^ 8  users
- No of users who post the content as per paretto principle: 20/100 * 3 * 10 ^ 8 users = 60 M users
- No of posts every day, per say 5 posts per user per day = 5 * 60M = 3 * 10 ^ 8 posts per day
- Hence the post creation that is write = 3 * 10 ^ 8 posts per day
- No of users who view the post content : 3 * 10 ^ 8 users.
- For every user see 100 posts per day = 3 * 10 ^ 10 posts per day
- Query per second: Here we are rate limiting hence the rate limiter can be write request or read request.
- Storage can be 30 bytes * 10 apis * 3 * 10 ^ 8 users = 90 GB data
- No sharding is required. We can manage this with internal storage of the machine. Hence we can achieve ultra low latency.
## Algorithm to implement rate limiter

```Java
// Brute force  
private boolean isReqAllowed(long userId, String api){
	
	// Getting coresponding queue for API and userId
	Queue<LocalDateTime> queue = getQueue(userId, api);
	
	// Getting current time
	LocalDateTime currTime = LocalDateTime.now();
	
	// Adding to the queue as it is requested
	queue.add(currTime);
	
	// Remvoing expired request timestamps
	while(!queue.isEmpty() && currTime - queue.peek() > 1min) queue.pop();
	
	// Check if the current request count is allowed
	if(queue.size() < 10) return true;
	else return false;
}

Cons:
- Lot of queues to maintain for different userId and apis
- Difficult to maintain many queues.  

// Time Bucketing with fixed sliding window
PROS:
- Easy to implement
- Less entries of data
  
CONS:
- Calculation between different buckets is not accurate
  
// Time buckting with approximation cross sliding calculation
PROS:
- Gives only 0.003% wrong rate limting values  

```

## Reference:![[WhatsApp Image 2026-02-22 at 6.14.22 PM.jpeg]]
![[WhatsApp Image 2026-02-22 at 6.14.22 PM (1).jpeg]]![[WhatsApp Image 2026-02-22 at 6.14.23 PM.jpeg]]![[WhatsApp Image 2026-02-22 at 6.14.23 PM (1).jpeg]]![[WhatsApp Image 2026-02-22 at 6.14.23 PM (2).jpeg]]![[WhatsApp Image 2026-02-22 at 6.14.23 PM (3).jpeg]]