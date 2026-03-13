#Freshworks #Round2 

## Challenges:
- **Scaling reads:** Scaling the reads for users who following many users and users whose post is being read by many followers.
- **Fan out reads**: Read request to multiple source
- **Fan out writes**: Writes post to multiple source
	- When user creates post have a entry of followerId, postId, userId so that when followers need feed we can compute feed from this table.
- **Scaling writes:** User with million of followers
	- Queues + async workers: We can have a queue to all the write request and the write workers can write those in DB.
	- Now DB is a bottleneck: For influencers we don't follow the queue approach instead when a user requests for feed we will get the normal posts from precomputed table and get posts from influencers and merge it. Async + hybrid approach.
- **Reading influencers post, hot key in DB:** Have it in cache so DB wont get high read load.
- **Not hot key problem in cache:** Replication can work here.
## System Overview:
- Facebook news feed which shows recent posts of other users the current user follow.
## Functional Requirements:
- User should be able to create posts
- User should be able to follow other users
- User should be able to view feed of posts of other users they follow in reverse chronological order.
- User should be able to scroll the feed infinitely.
## Non Functional Requirements:
- PACELC:
	- What is consistency?
		- When I can read the post immediately after the write of the post.
	- What is availability?
		- When user tries to create and save the new post, Is it okay to show unavailable due to some internal error.
		- We will miss out the post data.
	- Here availability is important more than consistency.
	- Eventual Consistency.
	- Low Latency
- Highly available.
- Low latency
- Eventually Consistent.
- Highly reliable 
- High durability.
## Scale Estimation:
- Daily active users: 3 * 10 ^ 8 users
- Using pareto principle:
	- Post creation: 
		- Users who post
## Core Entities:
- Users
- Posts
- UserFollowMapping
## APIs:
~~~
# Create Posts

POST /post

body:{
	content: "",
	media: "cdn URL",
}

# User should be able to follow other users
 
GET /users/{userId}/follow

# Get user feed informations 

GET /users/{userId}/feed?pageSize={pageSize}&timestamp={timestamp}

~~~

## High level design:
1. User should be able to create posts
	1. To achieve this we created a post service that helps to do the CRUD operations on Posts of the user.
	2. To store the post information we are using Dynamo DB as we have partition key and sort key combined as primary which help to perform range queries easily.
2. User should be able to follow other users
	1. We can have a mapping table that have following and followedBy relationship.
	2. This helps to find 
		1. What are all the users following particular userId
		2. What are all the users followed by particular user Id.
3. User should be able to view feed of posts of other users they follow in reverse chronological order.
	1. We can have a GET API that helps to get the recent posts of the users that current user follow.
	2. And in the request we can attach the timestamp where the user lastly scrolled so that, the next set of feed comes that are posted before the current post that we have, Feed in reverse chronological order.
		1. For current user
			1. Get all the users that current user is following
			2. Get all the posts posted by the users list.
			3. Sort the posts in reverse chronological order.
		2. This is high time consuming and high computational when the user list grows and user posts list grows.
		3. We think about this later.
4. User should be able to scroll the feed infinitely.
	- To achieve this only we made a GET api that having timestamp header in it, using which we are able to get the posts within requested time stamp when the user hits last post of current page.
	- We can get the next set of posts
**High level :**
![[Screenshot 2026-02-27 at 10.29.02 PM.png]]
## Deep dives:
1. How do we handle users who are following large number of users?
	1. This makes the post feeding logic high computational and time consuming.
		1. Why we need to recompute, we can add it to a table with user feed mapping when the post is created to fetch it easily.
		2. When the post is created, fetch all the followers of the user who posted it
		3. Insert a entry for userId and postId to get it in future.
		4. The above approach is called fan out on write.
		5. **CONS**: Any why this solved the read problem of news feed this created a write problem for post creation as what is the user with many followers posts something.
![[Screenshot 2026-02-27 at 10.38.25 PM.png]]
2. How do we handle users with large number of followers?
	1. When user will million followers creates a post, We again falls into fan out on write problem.
	2. Solution: Async pre computation feed creation
		1. We can make this precomputed entries creation async.
		2. When a new post is created we can create an entry in SQS with postID and creator userId.
		3. The feed worker nodes will get these entries and  create the required feed entry entry in the precomputedFeed table.
		4. CONS: But this results more work for feed worker node to create million of entries in the DB for users who have million of followers.
	3. Solution: Async pre computation feed creation with feed read
		1. We found a catch that our above approach will fail for users with millions of followers.
		2. Hence what we can do for those users who have millions of followers we skip the pre computation creation.
		3. While fetching feed on the request we will get the pre computed data and the current users following list user's recent posts that we skipped. So based on timestamp that will be less and gives good performance,
![[Screenshot 2026-02-27 at 11.05.53 PM.png]]
3. What if some posts are getting read multiple times and other posts are getting read 0 times
	1. This results in hot key issue where we need to handle as Dynamo DB cannot handle this read throughput as the throughput is uneven.
	2. Solution: Sharded Caching with most recent:
		1. We can have a write around cache with TTL invalidation strategy and LRU eviction policy.
		2. As the posts are rarely edited long TTL can work here.
		3. As long as the cache can hold the data, our cache hit will be more compared to cache miss.
		4. When posts are edited we simply invalidate the post ID.
		5. CONS:
			1. Here the problem is same hot key problem in cache now.
	3. Solution: Suffixing the postId .
		1. We can have these kind of most frequent posts that can have a attached suffix id and have this posts in all the shards 
		2. In this way we can distribute the read through put.
![[Screenshot 2026-02-27 at 11.18.21 PM.png]]