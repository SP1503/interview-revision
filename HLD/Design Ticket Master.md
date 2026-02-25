## Understanding the problem:
- Ticket master is an online platform that allows users to purchase tickets for 
	- concerts
	- sports events
	- theaters
	- other live entertainments

## (MVP) Functional Requirements:
- User should be able to search for events
- User should be able to view an event information
- User should be able to book tickets to events.

## Non Functional Requirements:
- **PACELC**:
	- What is consistency here mean?
		- Here consistency is defined as booking ticket information of user.
		- Hence if the booking information is lost then we lost the business, Hence consistency is important.
	- What is Availability here mean?
		- ~~My application should be available to book the ticket but consistency is not important~~
		- The above statement is false. Consistency is more important here.
- The system should be highly consistent for ticket booking.
- The system should be highly available for event searching and viewing events.
- The system should be able to handle high throughput for popular events.
- The system should have low latency search.

### Scale Estimations:
- Daily active users of our system is: 100 Million users = [10^8 users].
- Viewing and searching event estimation: [10^8 users].
- Booking estimation following 80:20 rule. 
	- 20/100 * 10^ 8 = [2 * 10 ^ 7] users will book tickets for our system.
- Each row of booking and ticket may consume around 50 bytes.
- How much data to write: [2 * 10 ^ 7 * 5 * 10 = 1TB] data we will be storing per day 
- Booking writes are less compared to Event reads.
- Hence it is read heavy application, Hence should be supporting high read throughput.

### Planning
- going one by one through Functional Requirements
- Then satisfy your non functional requirements
### Defining the core entities:
- Users
	- Users who are trying to book tickets for the events.
- Events
	- Stores the details of events like
		- Date
		- Description
		- Type
		- Performer Id
		- Venue Id
- Performers (Many to Many mapping with Events)
	- Stores the performers of the event they are performing
	- We can store
		- Name
		- Description
		- Profiles and links of the performer.
- Venue
	- Represents the physical location of the event.
	- Stores
		- Address
		- Capacity
		- Specific seat map
- Seat mapping
	-  When new event is created a new seat map will be created contains information about the booked seats and seat map.
	- This data will be used by the client to show the seat map to the customer while booking tickets.
- Booking (One to one mapping with User, Booking and Ticket having many to many relation ship)
	- Stores information about the 
		- UserId who trying to book ticket 
		- Booking status
		- total price
		- ticketIds
- Tickets
	- Stores information about event and user who booked ticket for the event
	- This contains
		- EventId
		- Seat Details
		- Pricing
		- Booked Status
- Payment

### APIS for system:
- View an Event
	- GET /event/{eventId}
- Search for event:
	- GET /events/search?keyword={keyword}&start={start_date}&end={end_date}
- Book ticket
	- POST /bookings/{eventId}
	- body: 
		- {ticketIds:[],  paymentDetails: }

### High Level Design of System

#### View Event:
- Client
	- User will interact with the system using client like mobile app, web browser ect.
- API Gateway:
	- Entry point for clients to access the different microservice through URLS.
	- Responsible for
		- Routing
		- Authentication
		- Rate limiting
		- logging
- Event service:
	- Our first microservice for handing view API requests by fetching necessary information from the Database.
- Event DB
	- Stores tables for events, performance and venues.
- View event walkthrough

#### Serach Event:
- Once the user logged in they are supposed to search for events based on
	- Event name
	- Artist name
	- Teams name
	- Location
	- Date
	- Event type
- This might be a parameterised search for the events.
- Search service;
	- To handle the search we will implement a new search service which speaks to DB for searching using LIKE keyword
	- This is slow and cumbersome, but this can be a goos starting point.
- Search Event overview

#### Booking Events:
- Main things to avoid is
	- Two users trying to book the same ticket 
- To avoid this consistency issue we are using a DB with Transaction support liek ACID properties
	- A : Atomicity - All or nothing
	- C : Consistency - Data will be consistent for any number of reads. This have levels
	- I : Isolation levels -The transaction that takes please in an isolated manner using locking mechanisms like
		- Shared locking - Reads are allowed but while writing no one can read
		- Exclusive locking - No reads and writes are allowed while writing
		- Read un committed:
		- Read committed
		- Repeatable Reads
		- Serialisable
	- Durability: How long the data is durable. How long we can keep the data with us.
- Create new tables like booking and tickets
- Booking service: Used to book tickets for events and interact with DB and 3rd part service for managing ticket booking
- Payment Processing: An external service responsible for handling payment transactions.

### Reference:
![[Screenshot 2026-02-25 at 7.37.22 AM.png]]
### With the above idea the pain points includes 
- Search of events will be complex query process
- User will be try to book tickets and giving payment information just to know it is booked.

### Deep dives:
1. How do we improve the ticket booking experience by reserving tickets?
	- Here at a given moment t1 two users tries to book same ticket
	- At that moment  request for one user will be successful who first meets the api gateway
	- But other will get to know that the ticket is booked which is a bad user experience.
	- **Solutions**:
		- **DB level locking**
			- We can lock the particular seat row until the booking is done, either successful or failure.
			- Once it came to that state we can proceed other to process the ticket.
			- Transaction success case:
				- Once the transaction is success the lock will be released, hence the other user get to know that the ticket is booked.
			- Transaction failure:
				- Here if the user transaction is failure or user not proceeded to buy the ticket then, our ticket will be blocked infinitely and rely on time outs to release the lock.
			- Cons:
				- Increased wait times for the users
				- Locks for long time like 5 minutes can strain the DB and increase the risk of
					- Deadlock
				- User can see an error instead of waiting to know that a ticket is booked or not
		- **Add status and set expiry:**
			- Adding a status column in the ticket table and setting expiry column in that table
				- Flow:
					- Once user requests
						- Change the status of the ticket to reserved with expiry as current time + 10 min
					- While the user purchased the ticket:
						- We changed the status of the ticket from reserved to booked.
					- If the user purchase is not successful then,
						- After the expiry time we can change the status of the ticket to AVAILABLE again.
				- Cons:
					- CRON job can be used to do the above. But there are some issues
						- We define the CRON job to run at particular time interval
						- Let say the expiry was set to 11:10 and the CRON job will run for every 5 min then last ran time is 11:9. then next CRON will be by 11:14 which makes the delay of ticket release by 4 minutes.
						- If there is any CRON job failure then ticket becomes AVAILABLE state is not possible. Tight coupling the things.![[Screenshot 2026-02-25 at 8.20.31 AM.png]]![[WhatsApp Image 2026-02-25 at 8.25.59 AM.jpeg]
		- Distributed Locking system:
			- AT the point of reserve we will add the reserve entry in the redis with key as ticket Id and value as booked with Time to live as 10 min
				- Ticket booking success
					- A new Booking entry will be added in DB.
					- The redis entry will be removed.
					- View event will take all the available tickets from the DB and check with redis for any reservation tickets, then proceed with showing it
				- Ticket on hold 
					- Even after 10 min the ticket is not booked then redis will remove the entry from it as TTL is happened.
					- 
				![[Screenshot 2026-02-25 at 8.42.21 AM.png]]
2. How to support view Apis for 10 ^ 8 users with concurrent requests:
	- Introducing:
		- Load Balancers
		- Horizontal Scaling
		- Caching
			- Caching the event information in redis cache.
3. How to have good user experience during high demand?
	- With popular event the loaded seat maps will be stale in short time as the ticket booking is more fast.
	- Good solutiuon: Introducing SSE real time seat updates, unidirectional
	- GreatSolution: 
		- Implementing an admin enabled virtual queue manages user access in high demand time
		- User request to see the booking page
		- User request sits in a queue
		- On ticket booking or session expiry we can dequeu the users from the queue
4. How to optimise the search ?
	1. We can have Elastic search which will token all the strings and create a hash map with string key and event as values
	2. Good solution: Full text indexing in DB. Slow and not more performance
	3. Great Soltuion: Full text search like Elastic search
5. How to speed up the frequently repeated search queries?
	1. Good: We can have Cache layer on top of the elastic search. AWS open elastic search provides node query caching automatically that can be used to reduce load in the elastic search.
	2. Great: Have CDN for storing popular search results in it. It is fast as it is geographically available edge servers.
