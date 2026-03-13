#Freshworks #Barraiser 

## Challenges:
- Scaling reads: Hot key problem when trying to read same key by multiple users, Usinf suffix with multiple shards.
- **Highly available and Fault tolerance:** Master slave replication can work, But here read throughput = write throughput, Hence we can change to multi master replication with gossip protocol for data consistency.
- **Scalable read and write:** 
	- Can introduce sharding as 1 machine = 24GB, we need 1TB storage. 1TB / 25GB = 43 with buffer 50 nodes we need to store 1 TB.
	- **Hot key problem:**
		- Hot Read:
			- Having read replicas but data redundancy is high
			- Have copies of hot keys in different shards.
				- Read: read from every shard and aggregate.
					- user:123#1 -> Node 1
					- user:123#2 -> Node 2
					- user:123#3 -> Node 3
				- Write: System update all the copies to remain consistent.
				- The above approach only for hot read. Hot for both read and write this approach wont work.
		- Hot Write:
			- Write Batching + Aggregation: Write the same key only one for 100ms.
				- Effective for counters, metrics where final state matters more than individual update.
				- Tradeoff: Batching delay and write visibility.
				- Not suite for immediate write visibility.
			- Suffix hot key write: Add suffix and write in any node using consistent hashing, But increased complexity in reading the same data from different shards.
- **To reduce latency between different redis shard:** Connection polling
- **Equal load distribution:** Consistent hashing. Results in equal load distribution and minimal load transfer.
## System Overview:
- Stores data as key value pair in memory across multiple machines in a network.
- Can do horizontal scaling across many nodes to handle massive workloads
- Cache cluster works together to partition and replicate data
	- Ensuring
		- High availability
		- Fault tolerance

## Functional Requirements:
- User should be able to 
	- Set
	- Get
	- Delete data in key value pairs
- User should be able to configure expiry time for key value pairs.
- Data should be evicted as per LRU policy.

## Non Functional Requirements:
- Storing 1 TB of data and expect to handle 10^5 requests per second.
- Is we need to shard ? 1 TB is  huge for RAM to process, Hence need to shard.
- Is we need the data to be durable ?  Yes it needs to be hence we need replication.
- **PACELC**:
	- **What is consistency here ?**
		- The data that user writes into the DB are the data that will be cached. Immediate Consistency can cause the data loss or 2 phase protocol needs to be done but that is costly as per operation.
		- Here eventual consistency is acceptable.
	- **What is availability here ?**
		- When we get some data to write we need to cache it so that it can improve read throughput.
		- Availability is important.
	- **In case of partition tolerance what is more important to us?**
		- Availability is more important compared to availability.
- As the system is already having eventual consistency hence we can accept low latency.
- The system should be scalable.
- The system should be low latency < 500ms

## Scale Estimation:
	- Need to support 100K requests per second
	- Need to support 1TB of storage

## Core Entities:
- We need to store key value pairs in our system.
- Hence we need Pair entity with key and value.
## APIs:
```
Set:
POST /{key}
{
	"value": "....." 
}

Get:
GET /{key} -> {value: "....."}

Delete:
DELETE /{key}
```

## High level design:
- We just need to create a class that have the set data in RAM and we can get that data structure we created
- The class supports 
	- Set(key, value)
	- Get(key)
	- Delete(key)
1. **User should be able to configure the expiry for each key and value they are giving** 
	1. we can configure a TTL for every new key value pair that we are creating.
	2. Problem with this approach is
		- When the expired data getting called with get() method then only expired values will get removed.
		- To remove this behaviour we are adding a CRON job to remove expired data with some given time schedule.
			- This CRON job runs for fixed time schedule and when the memory limit hits.
		- Now our TTL works but when the caching is full we need to evict the data as per the LRU policy.
``` java
class ValueInfo{
	
	int value;
	
	LocalDateTime expiry;
	
	ValueInfo(int value, LocalDateTime expiry){
		this.value = value;
		this.expiry = expiry;
	}
}

class Cache{
	
	private Map<Integer, ValueInfo> cacheData = null;
	
	public Cache(){
		this.cache = new HashMap<>();
	}
	
	public int get(int key){
		ValueInfo currVal = this.cacheData.get(key);
		
		if(currVal.expiry > LocalDateTime.now()){
			this.cacheData.remove(key);
			return null;
		}
		else return currVal.value;
	}
	
	public int set(int key, int val, LocalDateTime ttl){
		ValueInfo currVal = new ValueInfo(val, LocalDateTime.now() + ttl);
		this.cacheDate.put(key, currVal);
	}
	
	public void delete(int key){
		this.cacheData.remove(key);
	}
}
```
2. Data should be evicted from the cache as per LRU policy
	1. Map is used here to get the data in O(1) time, But it is not maintaining any order.
	2. If we have data in list we can maintain order but we are not able to get data in O(1) time
	3. Comparing both data structures
		1. Here when creation is requested
			1. Check if the key is already existing
				1. If yes, remove that node from the doubly linked list.
				2. Update the value with new TTL
				3. Add that node in the tail of the linked list to maintain order.
			2. If the key is non existing
				1. Then check the capacity of the cache.
					1. If the cache is full 
						1. then remove the first item in the doubly linked list.
					2. Create new node with key, value and its expiry.
					3. Add this node into the hash map
					4. Add this node to the tail of the linked list.
``` java
class ValueInfo{
	
	int value;
	
	LocalDateTime expiry;
	
	ValueInfo(int value, LocalDateTime expiry){
		this.value = value;
		this.expiry = expiry;
	}
}

class ListNode{
	
	int key;
	ValueInfo val;
	ListNode prev;
	ListNode curr;
	
	ListNode(int key, ValueInfo val){
		this.key = key;
		this.val = val;
	}
}

class LRUCache{
	
	private Map<Integer, ValueInfo> cacheData = null;
	private int capacity;
	private ListNode head;
	private ListNode tail; 
	
	public LRUCache(int capacity){
		this.cache = new HashMap<>();
		this.capacity = capacity;
		this.head = new ListNode(-1, null);
		this.tail = new ListNode(-1, null);
		
		this.head.next = this.tail;
		this.tail.prev = this.head.next;
	}
	
	public int get(int key){
		if(this.cacheData.containsKey(key)){
			ListNode currNode = this.cacheData.get(key);
			return currNode.val.val;
		}
		else return -1;
	}
	
	public int set(int key, int val, LocalDateTime ttl){
		if(this.cacheData.size() == this.capacity) evictCache();
		insertAtEnd(key, val, ttl);
	}
	
	public void delete(int key){
		remove(key);
	}
	
	private void remove(int key){
		
		ListNode currNode = this.cacheData.get(key);
		
		ListNode prev = currNode.prev;
		ListNode next = currNode.next;
		
		prev.next = next;
		curr.prev = prev;
		
		this.cacheData.remove(currNode.key);
	}
	
	private void evictCache(){
		
		ListNode currNode = this.head.next;
		
		ListNode prev = currNode.prev;
		ListNode next = currNode.next;
		
		prev.next = next;
		next.prev = prev;
		
		this.cacheData.remove(currNode.key);
	}
	
	private void insertAtEnd(int key, int val, int ttl){
		
		ListNode currNode = 
			new ListNode(key, new ValueInfo(val, LocalDateTime.now() + ttl));
			
		ListNode prev = this.tail.prev;
		ListNode next = this.tail;
		
		currNode.prev = prev;
		currNode.next = next;
		
		prev.next = currNode;
		next.prev = currNode;
		
		this.cacheData.put(key, currNode);
	}
}
```

## Deep dives:
1. How to ensure our cache is highly available and fault tolerant ?
	1. **Synchronous Replication(Master slave with immediate consistency):**
		1. Create few replicas of each shard.
		2. When write comes we writes all the replicas synchronously and then respond to client.
		3. This approach gives high latency but gives immediate consistency.
		4. **CONS**:
			1. If any replica goes down we are not able to have proper write operation which makes our system less available.
			2. Adding more replica is adding more latency and delays and failures.
		5. Immediate Consistency + High latency.
	2. **Asynchronous replication(Master slave with eventual consistency):**
		1. Update only one available replica at the given time synchronously
		2. But trigger the write to other replicas asynchronously.
		3. This will reduce the latency and give durability to the data.
		4. This eventual consistency to our system
		5. **PROS**:
			1. Enables better write performance
			2. High availability
			3. System scales better with additional replicas
		6. **CONS**:
			1. There can be some stale data in the replicas.
			2. Using 2 phase commit
		7. Eventual Consistency + low latency.
	3. **Multi Master replication:**
		1. Here all the nodes are same both write and read replicas
		2. Changes made in one replica is will be propagated to other replicas using gossip protocol.
		3. Provides good eventual consistency and high availability and low latency.
		4. **CONS**:
			1. Conflict resolution
			2. Connection maintaining with different peer replica is difficult.
2. How to ensure our system is highly scalable?
	1. Here the data limit is 1 TB
	2. We cannot have RAM to process 1 TB data or the queries will be slow.
	3. Hence we need horizontal partitioning ie sharding.
	4. Let say a machine can process 20K requests per second, we need 100K requests
		1. Hence 100K / 20K = 5 nodes we need.
		2. Buffer addition would be around 8 nodes.
		3. AWS instance = 32GB RAM we can use 24GB RAM out of it.
		4. 1TB / 24GB = 43 nodes for storage. buffer with 50 nodes.
	5. Total we need 50 nodes to process this.
3. How to distribute all the required load to the 50 nodes ?
	1. Consistent hashing
	2. Reference:![[Screenshot 2026-02-25 at 9.48.33 PM.png]]
4. What happens if we have a hot key that is getting read for a lot of times ?
	1. Hot key: key that is considered for large number of times having high traffic
		1. **Hot reads**:
			1. Keys that receive high volume of read request like viral linked in posts.
		2. **Hot writes:**
			1. Keys that requests many concurrent write requests for the same keys. Like voting for TVK repeatedly.
	2. **Solution**:
		1. Replication: Here the keys needs to be read by multiple users. hence this requires to be present in multiple read replicas.
			1. **PROS:**
				1. The data will be available by default in all the read replicas.
			2. **CONS:**
				1. This can introduce eventual consistency
				2. Only for small data we are replicating every thing into a new replica with machines.
		2. Copies of hot keys
			1. Monitoring all the keys and identifying the hot keys
			2. Creation different versions of keys
				1. user:123#1
				2. user:123#2
				3. user:123#3
			3. The above copies gets distributed to different shards using load balancer.
			4. For reads client automatically get these data from different replicas.
			5. For writes maintaining consistency across different shards is difficult.
			6. Hot reads are okay but hot writes are not consistent.
			7. PROS:
				1. The hot key read load is distributed using the shard and load balancers.
			8. CONS:
				1. The main challenge here is how we are going to sync the data of these hot keys across nodes.
5. What happens if you we have hot key write that is being to a lot ?
	1. Write Batching:
		1. Instead of redirecting all the write queries to the hot example 100K writes, we can batch those writes that came in last 100ms using some atomic operation, we can reduce the write load to the cache.
		2. This helps in case of counters, voting system and video likes.
		3. PROS:
			1. Write load is less compared to before as we are batching now.
		4. CONS:
			1. Small delay in the execution of write accuracy but that is acceptable.
			2. Complexity of handling failures during batch windows.
				1. If the batch processing fails we need mechanism to recover.
	2. Sharding hot keys with suffix
		1. Splitting hot keys into different hot keys by adding suffix
			1. views:video123 into
				1. views:video123:1
				2. views:video123:2
				3. views:video123:3
				4. views:video123:4
				5. views:video123:5
				6. views:video123:6
			2. Have these in different shards 
			3. When write comes add in any of the shard.
			4. PROS:
				1. This distributes the write load across different shards.
				2. For example: 6000 requests/seconds is distributed into 1000 requests/ shard.
			5. CONS:
				1. Increased complexity of read operations which now we need to sum from multiple shards.
				2. This increases the read latency for the hot key.
				3. Too few shards wont handle distributed shards and high number of shards will increase the read latency.
6. How do we ensure cache is high performant?
	1. **To have high availability and fault tolerance:** we added replication with 
		1. Asynchronous replication: writing only to master and a single replica. Perfect balance between latency and consistency.
		2. Multi master replication as writes happens in every node and there is a gossip protocol we can have between them. 
	2. **To ensure cache is scalable:**
		1. We introducing sharding with load balancer having consistent hashing technique.
		2. Based on RAM: We need to store 1TB of data in RAM. In general AWS instances stores 25GB RAM out of 35 GB RAM
			1. 1025GB/25GB = 44 nodes.
		3. Based on throughput: we need to handle 100K requests, each request can handle 20K request in max
			1. Node requirement = 100K/20K = 5 nodes  with buffer we have 8 nodes.
	3. **To have even distribution of keys:** we added load balancer with consistent hashing algorithms always for stateful servers.
	4. To handle hot key read:
		1. Having multiple read replica: 
			1. Read load is distributed but only for small set of hot keys we are having more replicas.
			2. Copies of hot keys in sharding:
				1. Add suffix to the hot keys and have those in the different shards.
				2. Consistent hashing will take care of read distribution.
				3. But here data sync up is a challenge.
	5. To handle hot key write:
		1. Batching writes as sliding window having write requests of 500ms.
		2. Sharding hot keys with suffix:
			1. Attaching suffix in keys and distribute to different shards.
			2. Read need to sum all the values of different shard.

## Reference:
![[Screenshot 2026-02-26 at 9.37.48 AM.png]]