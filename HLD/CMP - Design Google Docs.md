
## System Overview:
- Google Docs is a web based collaborative document editor.
- User can create document s and edit those collaboratively in real time.

## Functional Requirements:
- User can able to create document.
- Many user can able to edit the same document concurrently.
- User should be able to view each others changes in real time.
- Users should be able to see other users cursor position and their presence in the document. 

## Non Functional Requirements:
- CAP:
	- What is consistency ?
		- The edited text data should be visible to other users after some time < 200 ms.
	- What is availability?
		- When user providers some edit the system should accept those edits.
	- Here availability is important compared to consistency.
- PACELC:
	- Consistency is already compromised.
	- Hence we can choose low latency here.
- The document state should be eventually consistent
- Updates should be of low latency < 100 ms
- The system should scale to 10^6 of concurrent users across 10^9 documents.
- No more than 100 concurrent users can edit a single document.
- Document should be durable and available.

## Scale Estimation:

## Core Entities:
- Users
- Documents
- Document Operations
- Cursors

## APIs:
~~~

1) Document creation:
   
POST /doc

request:
{
	title: String
}

Response
{
	documentId: 
}

2) Real time edit
High frequency calls with low latency and bidirectional

SEND
{
	type: "insert" || "delete"
}

RECV
{
	type: "update"
}

SEND{
	type: "updateCursor"
}
~~~

## High level design:
1. **User should be able to create documents ?**
	1. Client: trigger a REST POST api call to create a new document with document name.
	2. Api gateway: Used to route the request to the service that handles it.
	3. Document Metadata service: Used to perform CRUD operations on document metadata.
	4. Document Metadata Database: Used to store metadata about documents.
	5. **Reference**:![[Screenshot 2026-02-26 at 7.27.28 PM.png]]
2. Multiple user should be able to edit the same document concurrently
	1. Here multiple users will try to make high frequency edits to the same document at the same time.
	2. Consistency problem: 
		1. Active users tries to send insert message at the same index, if we collapse the order of execution the data both will have is not consistent. 
		2. Hence the data between two active users are not consistent.
	 3. Contention problem:
			1. Multiple threads tries to access the same resource at the same time,
	4. How to solve this consistency problems
			1. Passing the entire snapshot to the document processor
				1. Always data load getting passed as request.
				2. Consistency issue is still there.
			2. Sending Edits
				1. word = Hello!
				2. User A adds word = INSERT(5, ", world")
				3. User B deletes word = DELETE(6)
				4. User A executes before user B = Hello, World
				5. User B executes after User A = Hello, World! -> Hello World
				6. Hence the order on execution creates problems
				7. Solved by 
					1. OT - (Operational Transformation)
						1. Low memory and fast
						2. Having a central server to process the changes to change the operations based on the previous operation 
						3. CONS:
							1. Need central server to process the final ordering of oeprations.
							2. This will not help us to scale for large number of users.
							3. Ot is tricky to implement and easy to get wrong
					2. CRDT - (Conflict-free replicated data types)
**Sending Whole text as request:**
~~~

Hello!

User A submits Hello, World
User B submits Hello

Both happends at the same time.

Since the blog storage is transactional, Last update only retain in DB.

Output: Hello -> this is the output UserA chnages is lost.
~~~

OT explanation:
~~~

current = "Hello"

User A insert(3, x) = Helxlo -> expected

User B delete(3) = Helo -> expected

--------------------------------------------------------------------------------
BEFORE OT:

On concurrent execution:
Initial = Hello!

User A requests: insert(5, ", world")
- Here the output = Hello, world!

User B requests: delete(5)
- Here the output = Hello World!
  
user B's intent is to delete the exclamatory mark.

Output changed
---------------------------------------------------------------------------------
AFTER OT implementation:

On concurrent execution:
Initial = Hello!

User A requests: insert(5, ", world")
- Here the output = Hello, world!

# the OT transforms the request to
User B requests: delete(12)
- Here the output = Hello, World
  
user B's intent is to delete the exclamatory mark which is done here.

Output changed
~~~
**Reference**:
![[Screenshot 2026-02-26 at 9.03.29 PM.png]]
3. User should be able to see each others changes realtime
	1. Two possibilities:
		1. Both users are online
			1. Both users will have their own web socket connection.
			2. All these connections are terminated at same document servers.
			3. When a user makes a edit the edit will get saved in DB and then document service will broadcast the edit  to other web-socket connections to have the edit to other clients.
			4. We need to do OT at client level as well as for better UI the clients changes will be applied to his version first and then only other client edits are applied.
			5. So the data will not be consistent.
			6. Here we, to manage the concurrent updates by taking the sequence of edits in arbitrary order and rewrites them so they consistently returns the same document on the client side.
		2. One User is online and other users are offline:
			1. The changes made by one user will be saved to the storage
			2. When the other users comes to online, they connect to the same document operations service using web socket
			3. In that case, all the non updated changes will get pushed from DB to the client using web socket connection.
		**Reference:**
		![[Screenshot 2026-02-26 at 9.21.54 PM.png]]
4. User should be able to see the cursor positions and presence of other users 
	1. As per the requirement we only care only about where the other collaborators cursor currently now and not where one hour before.
	2. We can store the cursor positions in the document in in memory 
	3. This makes the service stateful. 
	4. Hence we need to use consistent hashing load balancer to have this

## Deep dives:
1. How do we scale to millions of web-socket connections ?
	1. We need to scale the document service to the number of concurrent connections.
	2. I need to know in which document service my all other collaborators are connected
	3. There I need to connect.
	4. When a client need to connect
		1. They open a HTTP connection to any of the document service with requested document Id
		2. The server will check the zookeeper to get the server id for the provided documentId and If the current server is not the server id requested then the request will be redirected.
		3. Once the correct server is found, the HTTP connection is upgraded to the web socket connection
		4. All the operations will be pushed to the socket so that the client.
2. How do we keep the storage under control ?
	1. Let say each document is 50KB then Billion of documents = 10 ^ 9 * 10 ^ 3 * 50 = 50TB of storage
	2. Here the operations are more so that storage grows up to 50TB
	3. We dont need to store all the operations
		1. Document service periodically snapshots operations.
		2. When the document is not connected with any connections
			1. Take all the existing operations and offload them to a separate process of compaction
			2. Write the resulting operations to the DB under new document version Id
			3. Flip the document versionId into the document meta data
**Final document Reference:**
![[Screenshot 2026-02-26 at 10.28.02 PM.png]]