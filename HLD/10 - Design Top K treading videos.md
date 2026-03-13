#Round-2 #Freshworks 
## System Overview:
- This system provides a feature to see the top K videos of any timeframe.
## Functional Requirements:
- System should be able to get the top K treading videos for the given time frame.
	- Top k now
	- Top K for yesterday - startTime and EndTime
	- Top k for last week
	- Top K for last month.
- Here we are supporting Tumbling windows that is week(7 days), month(28 days)
## Non Functional Requirements:
- CAP theorem:
	- Consistency: When the views of video is happened and it gets tabulated in top K, we can have 1 minute delay. Eventual Consistency.
	- High Availability to view top K videos in the given time frame but that can be stale < 10ms
	- The read latency should be less than 10 ms
- Scalability:
	- Can store around 1B(10^9) videos.
	- 100K views per second
- The views values are to be precise.
## Core Entities:
~~~
Videos
Views
TimeWindow
~~~
## APIs:
~~~
#API to view top K videos in the given time frame

GET /views/top-k?window={window}&k={k}

Response:
{
	{
		videoURL: urls
		views: count
	}
}

# Update API to update the count of particular Id
PATCH /video/{videoId}
{
	count: count
}
~~~

## High level design:
1. **Client should be able to query the top K videos of all times**
	1. We having a Kafka queue with topic view event that is holding all the videoId with views of it.
	2. We can have our view consumer to consume these informations and have it in views DB.
	3. We can have view index to allow range queries based on views.
	4. We can introduce the Top K view service to get the top K results from the DB.
	5. The cost we give is that for every write instead of executing in O(1) we are going to update the index of view that going to cost us O(log N).
	6. Query: ==SELECT videoId, views FROM views ORDER BY views DESC, LIMIT K==
2. **Client should be able to query the top k videos of a given time frame(startTime, endTime) that can be past hour, past day, past week, past month, past year.**
	1. In this case we are going to store the view count based on the one hour window. Because that is the less suitable unit of time we can mention from which we can calculate the top k views of rest of the time frames.
	2. Now we are updating the index with timestamp as again the range queries will become easy.
	3. Query: ==SELECT videoId, SUM(views) FROM views WHERE startTime >= {startTime} AND starTime <= {endTime} GROUP BY videoId ORDER BY SUM(views) DESC, LIMIT K==
## Deep dives:
1. **How can we cut down the number of queries to the DB directly ?**
	1. **Good solution:** 
		1. We can introduce redis to store the frequent data with different windows. what are all the possibilities
			1. 24 hour of a days
			2. 7 days of the week
			3. 4 weeks of the months
			4. 12 months of the year
		2. As we are storing only the video Id and the view count of total K elements let say every entry is 16 bytes and top 1000 on every category = 16000 which is 16KB.
		3. That is more less compared to others.
		4. **CONS**: even now the first query to find the Top k will reach the DB that will take time to process.
	2. **Great Solution:**
		1. We can precompute the top K for each window only for months and store it in cache for 28 days
			1. As we are getting count only for current hour.
			2. We will update current hour top K
			3. Current Day top K
			4. Current Week top K
			5. Current Month top K
		2. And we can run this cron jobs at unusual time which can impact less customers.
2. How can we handle the massive number of writes to the DB ?
	1. Scaling
		1. 70B views per day
			1. 70 * 10 ^ 9 / 10^ 5 = 700K views per second.
		2. A postgres write can handle only by 10K requests per second.
			1. We can partition the Kafka stream by a subset of videoId
			2. We can shard the view consumer by the same subset of videoId.
			3. We can shard the view DB by the same subset of videoId.
				1. I can handle only 10K write request per second
				2. Expectation is 700K request so we need 70 nodes or shards.
			4. 
		3. How much videos we will store
			1. 4Billion videos * 16 bytes = 64 GB total 70 shards of 64 GB is not huge.
		4. But as we sharded now executing the top K views query needs to be done on all the 70 shards and we need to merge all those calculations.
		5. Cost:
			1. 70 Machines
			2. 70 DB shards
		6. Instead of writing to database on each view we can aggregate the request to write for every one hour and we can give the aggregated request to the DB.
		7. We can use Apache Flink here to aggregate the request and give aggregated result. Flink is best for aggregation and batching
		8. Now flink goes down flink will get new host and will start processing from the missed time frame.
		9. 100 write request on same videoId = flink = 1 request with count as 100
		10. Hence by batching we can bring the number of shards from 70 count to 5 to 10.
3. How do we optimise our top K queries?
	1. Here we are making query to the DB to get the views count on 
		1. Hourly
		2. Daily
		3. Monthly 
	2. While generating data can we increment the view in the DB in 
		1. Daily
		2. Hourly
		3. Monthly basis
	3. In that case we can see out CRON job can straight forwardly add data to redis cache.
4. What if we need to support sliding window ?
5. Can we make use of approximations to improve performance ?

**Reference:**
![[Screenshot 2026-03-06 at 3.18.02 PM.png]]
