#Round-2 #Freshworks
### DDOS: Distributed Denial of service
## Challenges:
- Dealing with contention: More process try to update counter at the same time
- Scaling writes: Millions of counter updates per second using redis with Read, modify, write atomically. 
- Scaling reads: Millions of reads to same key introducing hot key problem solved by replication
- **Scaling writes to do rate limiting:** Shard the redis using client identifier parameter + consistent hashing
- **Fault Tolerance and High Availability:** Have replica for every redis instance.
- **Redis round trip latency:** Can have connection pooling to avoid TCP handshake time.
- **Scaling reads hot key problem in caching(Celebrity problem)**: Have a client side rate limiting like cloud flare to limit rate as we cannot use suffix distribution like what we done in social media.
- **Dynamic rule changing:** Have a centralised configuration manager like zookeeper - push based configuration.
## Overview of system
- System used to allow only x number of requests from a unique parameter within y timeframe.
- This helps to restrict the total number of requests a user can make within the given timeframe.
- The rest of the requests are being restrict with response code as 429 says too many requests.
- Helps in preventing
	- DDOS 
	- servers gets overwhelmed by the requests.
## Functional Requirements (MVP): features of the system
- User should be able to configure the constraints with parameters like
	- No of requests allowed
	- Timeframe
	- Parameter using which the client needs to be identified uniquely.
		- Id
		- Ip
		- api Key
- System should restrict the requests beyond configured limit with 429 response code.
- Should be showing the request analytics.
## Scale:
- 100M DAU
- 1M req per sec
## Non Functional Requirements: How it going to do
- CAP:
	- What is availability ?
		- If client send request to us we say some error to them.
	- What is consistency ?
		- Immediate read after write
		- Number of requests the user triggered as of now value.
- System should be [highly available] on saying whether the request is allowed or not
- System can have [eventual consistency] on number of request it triggered.
- System should have [low latency < 10ms].
- System should be scalable to 1M req per second.
- System need not to have request information beyond the given timeframe - less durability.

## Core Entities: What I will persist
- Rules
- Request
- Client

## API interface:
~~~
#Create rules for rate limiting
POST /rules

request:{
	timeframe: "",
	unit: "",
	parameter: "",
}

# Check if the current request is allowed to proceed
GET /isAllowed/{parameter}/rule/{ruleId}
~~~
## High Level Design
- Algorithm type:
	- Fixed window counter
~~~
  Hash map having request counts for each client at given time stamp
  {
	"A:12:00:00": 100,
	"A;12:00:01": 5,
	"B:12:00:00": 20,
	"C:12:00:00": 0,
	"D:12:00:00": 54,
	"E:12:00:00": 0,
	"F:12:00:00": 12		  
  }
~~~
- Sliding Window Counter
	- Gives some kind of better approximation.
	- CONS: it is a approximation
~~~ java
private int findRateCount(
	Map<Integer, Integer> prev, 
	Map<Integer, Integer> curr, 
	int currTime, 
	int timeFrame){
	
	int prevTime = timeFrame - currTime;
	int count = curr.get(client) + prevTime/timeFrame * prev.get(client);
	return count;	
}
~~~
- Token bucketing system
	- We can have a bucket for every individual client
	- We can fill those buckets with N tokens that are allowed within the given time frame.
	- We will refill those bucket only in the last fill time + time frame time.
## High level design satisfying the functional requirements:

![[Screenshot 2026-02-28 at 8.31.04 PM.png]]

## Deep Dives:
- Redis can have 50K requests per second.
- Hence required sharding.
- 1 * 2* 10 ^5 / 10 ^ 4 = 20 nodes
- **How do we scale up to handle 1M requests/second?**
	- A redis node can handle around 100K requests per second
	- But our requirement is to handle 1M requests = 10 ^ 6 / 10 ^ 5 = 10 nodes.
	- We require 10 shards to handle all the data
	- This distributed the data now again. Hence we need a consistent hashing to direct to correct shard.
	- But here is the key redis cluster will handle this distribution between sharding problem using hash slotting.
- **How do we ensure high availability and fault tolerance:**
	- Best solution is to Fail open
		- As for that particular moment we are rejecting the rate limiter.
		- But is any issue occurs it can cause the cascade failures.
- **How to minimise the latency ?**
	- There is a network overhead between the api gateway and the redis.
	- What if we have the persistent TCP connection for redis, we can able to reduce the TCP handshake time and allows connection to be reused.
	- Geographical distribution helps to reduce the network latency.
	- Have the redis cluster in the same data centre where we have the api gateway.
- **How to handle the hot key issues as redis can have celebrity problem ?**
	- We can temporarily block the userId that cross the rate limiting rules.
	- We can have that in the cache layer.
- **How to change the rule associated with the rate limiter dynamically ?**
	- Configure a push based model using zookeeper to push the rule when it is changed. 

## Design that handles all the NFRS:
![[Screenshot 2026-02-28 at 8.55.42 PM.png]]