## System Overview:
- It is a online platform which collects the logs from distributed servers and monitoring it in one place.
## Functional Requirements:
- User should be able to register a source of logs provider in the system.
- User should be able to see the realtime logs that are generated from the source.
- Logs needs to be normalised, validated and parsed as per the standards we have.
- User should able to see all the logs in the dashboard.
## Non Functional Requirements:
- PACELC:
	- What is consistency here?
		- The user should able to see the latest logs for a immediate read after write.
		- The logs needs to be updated in read time.
	- What is availability?
		- If any source provides the logs to the system, it should not reject it, instead it needs to be stored.
	- Here we prioritise availability over consistency.
	- In place of consistency and latency, 
		- We already compromised consistency. Hence latency can be prioritised.
- Scale: Millions of logs per hour.
- Latency: Low latency
- Reliable: No data loss.
## Scale Estimation:

## Core Entities:
- Sources
- Logs
- BatchJobs
## APIs:
~~~
# Register source for logs
POST /source/register

Request:
{
	
}

# Search alll logs or based on criteria
POST /logs/search
Req:
{
	pagination:
	criteria:
	filters:
	regex:
	timeframe:
}

# Add logs 
POST /files/upload
GET /files/{fileId}/status
~~~
## High level design:

![[Screenshot 2026-02-27 at 1.13.46 PM.png]]
## Deep dives:
1. What data base that we use to store the source of the logs in system?
	1. Here the source name and schema is structured and relational hence we can proceed with Postgres.
![[Screenshot 2026-02-27 at 1.44.33 PM.png]]
2. Here we will get millions of request to ingest logs, But we have only one service,In this case how we can reduce the load?
	1. We can divide our current ingestions log service into two service
		1. Agent based ingestion logs
		2. File upload based ingestion.
	2. Hence we have two service now.
![[Screenshot 2026-02-27 at 1.45.35 PM.png]]
3. Even now the ingestion write load will be high on the database of logs, What we can do here?
	1. Instead of directly writing the logs to DB, We can have a kafka message bus in between the producer(ingestion service) and the consumer(logs DB).
	2. This is batch the logs and provide the batched logs.
![[Screenshot 2026-02-27 at 1.47.24 PM.png]]
4. We need to validate, parse and normalise the logs that we have?
	1. To do that instead of having consumer as logs DB we can have Apache Flink that will
		1. parse the data
		2. normalise the data
		3. validate the data
		4. remove duplicated data
	2. and store that data in DB.

5. We need to have low latency search for logs ?
	1. here to reduce the search latency we can go for elastic search component
	2. Elastic search will get the data from the Apache flink 
	3. But elastic search cannot hold more than 14 days data, Hence larger than 14 days data can be fetched from the DB.
	4. And totally our system will store only logs of 30 to 45 days to reduce the storage in cassaandra and to increase the write throughput.
![[Screenshot 2026-02-27 at 1.52.49 PM.png]]
6. How we can scale the storage? can the elastic search hold all the data?
	1. No the elastic search is used to make the search faster only for data less than 14 days.
	2. Hence the rest data will be stored in DB.
	3. But DB also cannot store all the data in that case we need to store 30 to 45 days data in DB and rest data can be present in cold storage like s3.
	4. We can use a CRON job to delete data from DB after 45 days.
	5. In Elastic search we can configure a TTlL to expire.
![[Screenshot 2026-02-27 at 3.33.39 PM.png]]
7. What if We need to notify the user if any alert comes in?
	1. We can check if there is any error messages comes in 
	2. If yes Kafka can stream those messages to different topic let say alerts.
	3. There can be a consumer named alert service that will get the alerts and alert the people using a 3rd party notification service.
	4. Here in Alert service we can have schema that have the rule definitions on which type of error can be communicated using which mechanism.
![[Screenshot 2026-02-27 at 3.51.47 PM.png]]