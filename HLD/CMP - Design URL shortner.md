
## System Overview:
- We are given with a long URL, we need to convert it into a short URL and provide that in response.
- The user will use this short url to redirect to the website provided by the long url.
- Provides analytics for each URL present.
## Functional Requirements:
- User should be able to create short URL  for the given long URL.
- User should be able to redirect to the website of long URL through this short URL
- User should able to set expiry for the generating shorty URL.
- User can give their custom alias to this given URL.

## Non Functional Requirements:
- CAP:
	- What is consistency here ?
		- Data that it gives immediately read after write.
		- Hence it can be compromised.
	- What is available here?
		- The system should get the long URL when passed the short URL.
	- Here we prioritise availability over consistency.
- PACELC:
	- Here we already compromised consistency, Hence we can go with low latency.
- System should give unique short URL for every long URL we are giving.
- The redirect between the ULR should occur with low latency < 100ms
- System should be durable and available,
- System should be able to store data upto 1B shortened URLS in it.

## Scale Estimation:

## Core Entities:
- User
- UrlMapping

## APIs:
```
1) Create short URL

POST /url
body{
	longurl: "url",
	expiry ?: expiry,
	customAlias ?: ""
}

2) redirect to long URL
GET /{shorturl}
response -> Redirect to the original long URL with response code 302

```
## High level design:
1. User should be able to create a short URL for the given long URL.
	1. Client: Client to send request of the long url to create the short url.
	2. Api gateway: used to have routing mechanism
	3. Server: Used to get the request mapping and create short URL and store.
	4. Database: Used to store the mapping of long URL with short URL.
	5. Client provides the long URL as request
		1. Validates the URL using libraries
		2. Create short URL for anyURL even that is present in the DB as the expiry will be different.
		3. If custom alias is given and that alias is not present in DB, we can use that
		4. if present return response as error.
		5. TO protect future generating codes not equal to custom alias, we are appending a char prefix which custom alias cannot use.
		6. Store the short URL in  DB and send it as response.
2. User should be able to access the long URL using the short URL
	1. In this case
		1. user requests for the long URL using the short URL
		2. Server will get the URL from DB if the short code matches.
		3. If expiry is reached we can return 410 gone status.
		4. If found server can respond with long URL using the 302 response code.
		5. 301 is permanent redirect: 
			1. means browser will cache this response and next time it wont come to our server to get the long URL. 
			2. Here we are missing the analytics part.
		6. 302 temporary redirect:
			1. this response wont be cached in the browser.
			2. Helps to update or expire links
			3. Helps to perform analytics

## Deep dives:
1. How can w ensure short urls as unique?
	- **Hashing + Base62**
		- We can do hashing of the given long URL
		- Encode it using base 62 that gives the 62 ^ 8 = 218 trillion possible codes.
		- CONS:
			- Despite the randomness still there is a chance of generating duplicates when the system grows.
			- To avoid we need to make the short urls long that abides our agenda.
			- input_url = "https://www.example.com/some/very/long/url"
			  hash_code = hash_function(canonical_url)
			  short_code_encoded = base62_encode(hash_code)
			  short_code = short_code_encoded[:8] # 8 characters
	- **Hashing + Base62 + counter with base62**
		- To avoid collision for sure we can have a counter using redis distributed cache.
			- Redis is single threaded and perform operations in atomic manner.
			- Hence any two concurrent increment calls will give different values.
			- call 1 gives 1001 and call 2 gives 1002.
		- Which gives the incrementation when ever a new shortURL is created.
		- This counter also can be encoded using base62.
		- **CONS**:
			- No CONS
			- If we need to support more instead of 62^6 combinations we can use 62^7 combinations.
2. How we can ensure the redirect is fast?
	1. Indexing: 
		1. We can index the DB. 
		2. In postgres the indexing is happening using Btrees.
		3. Since here our primary key short URL is always unique we can search the short URL in log N time complexity.
	2. Having Cache layer:
		1. Here we can have a redis layer to increase the read throughput.
		2. Invalidation strategy: Write around cache - eventual consistency.
		3. Eviction policy: LRU cache.
		4. Redis: 100 Nanoseconds
		5. SSD: 0.1 milliseconds
		6. HDD: 10 milliseconds.
		7. CONS:
			1. Updation or deletion occurs take eventual consistency
			2. Initial request is always a cache miss.
	3. Have Cache + CDN:
		1. Since it is geographical, we can store them most used short URLS in the CDN.
		2. CONS:
			1. Analytics will be missed as the request will not reach backend server.
			2. Setting CDN increase the complexity of the architecture and costly.
3. How we can scale to support 1B shorted URLS for 10^8 DU?
	1. For a shortURL we use around 50 bytes.
	2. For 1B URLS = 10^9 * 50 = 50GB data is we can manage.
	3. To have the fault tolerance and durability, we can have replication with Master slave concept.
	4. We can also save the snapshot of this as a backup in Blog storage.
	5. For 10 ^ 8 DU means
		1. 10^8 / 10^5 = 10^3 requests per second.
		2. AWS instance can manage 10^3 but to be in safer side we can have 3 nodes to handle this.
		3. And process the counter in form of batch.

**Reference:**
![[Screenshot 2026-02-26 at 3.30.00 PM.png]]