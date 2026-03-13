#Freshworks #Barraiser 
## System Overview:
- Uber is a system that can be used
	- To find the near by cabs with different types of vehicles
	- Their fair estimate to travel from current location to destination
	- Request a ride with the estimated fair
	- Continue the ride

## Functional Requirements(Minimal Viable Product):
- User should be able to provide their start location and end location to find estimates on the cabs.
- User should be able to request a ride to a rider with the estimated fare. 
- Riders should be able to match with the nearest driver available. 
- Drivers should be able to accept/reject the ride request and navigate to the pickup point and drop point.
## Non Functional Requirements (System requirements):
- Low latency driver and rider matching < 1 minute if not failure.
- CAP theorem:
	- Consistency: the system should be consistent on fro which driver which rider is mapped and it should be a 1:1 matching.
	- Availability: 
		- The system should be highly available for fare estimations.
		- Highly available 
- High throughput in times of surges for peak hours or special events, 100K requests from same location.
## Core Entities:
- Rider
- Driver
- Location
- Ride
- Fare
## APIs:
~~~
- POST /ride/fare-estimate - create ride and get fare-estimates
{
    source,
    destination
}

PATCH /ride/request - find driver matching and assign
{
    rideId,
    vehicleType:
}

PATCH /driver/location/update - update continous location of driver
{
    latitude:
    longitude:
}

PATCH /driver/ride/accept - driver accepted or rejected the request
{
    rideId:
    driverId:
    accept:
}

PATCH /ride/update - status update about the rifer
{
    rideId:
    status: "pickedUp"/"dropped"
}
~~~

## High level design:
- **Need**
	- Client(Driver, Rider)
	- Api Gateway (Routing, Authentication, Authorization, Rate limiting)
- **Rider should be able to input start and end location and get estimated fares for different vehicle types**
	- Ride service:
		- To create rides with possible fares and with different states.
		- Dynamo DB can be used for ride service.
	- 3rd party mapping API
		- We need to find the fare for the source and destination.
		- For that mapping purpose we can use a 3rd party mapping API to get the kilometer and calculate a fare.
- **Rider should be able to request ride to nearby drivers who are all available.**
	- Ride matching service
		- This service is highly computational hence segregating into different DB.
		- This service will get the list of drivers available nearby and request them to accept orders one by one with some wait time.
	- Location DB
		- Used to have the Latest location information about the drivers.
	- Driver client:
		- The client that drivers have, should update the location of the driver periodically.
		- Those locations will be saved in location DB.
	- Location Service:
		- As the location update will have high through put as we are updating for every users for a periodical time, we need to handle that as a separate service.
- **Driver should be able to accept or reject the ride request based on their perspective**
	- Notification service: on the behalf of ride request, the service can able to notify the driver for a ride request. Hence we need a notification service.
	- On the notification as the response, the driver should be able to accept or reject ride. Hence driver client can trigger a update API call to ride matching service.
## Deep dives:
- **How to handle high frequency of location updates about the driver and efficient nearby location searches?**
	- High frequency location updates:
		- Approximations of 6M drivers
		- 3M drivers are active
		- We are updating the location of the user in every 5 seconds.
		- Total updates = 3M drivers / 5 = 600K requests per second.
		- Postgres cannot handle
			- As it can handle only 50K requests per second. But the requirement is 600K requests
			- Finding the distance between the drivers and riders query can be higly inefficient as we need to look up every row and the indexing of B+ trees for this wont work
		- Caching can handle 100K to 1M requests per second. Hence we can have a redis cache that supports geohashing.
	- Better Idea: Batching the writes for a period and Geospatial databases (quad trees)
		- This can reduce the write throughput as we are aggregating the writes for a particular period of time.
		- geospatial database can be used as it specialised to store the two dimensional datas like latitude and longitude.
		- CONS:
			- As we are batching the writes the data stored in the DB will not be a accurate data of the driver location. We lost the purpose.
	- Great idea: Use redis based geospatial database 
		- Cache layer like redis can exhibit high write throughput.
		- Reduced the storage space with automatic data expiry.
		- Using geohashing to encode latitudes and longitudes.
			- This is perfect for high frequency updates of data.
			- Good for even distribution.
		- CONS:
			- Data durability
				- We can enable to redis persistence to store the data of the redis periodically,
				- We can enable redis sentinel for high availability of redis clusters using master slave concept.
 - **How can we reduce the high frequency location updates that can make system overload?**
	 - We can reduce the high frequency writes of location into DB by
		 - We can adjust the frequency based on
			 - Distance
			 - Driver status like idle or stop.
- **How we can prevent multiple ride request that can be sent to same driver ?**
	- We can request the driver one by one at the same time not asynchronously but what if in horizontal scaling the same driver gets pinged for different riders as they are in same location.
		- This can be avoided by using a distributed lock where we will lock the driver id when a machine is requesting them and after 10 sec if driver accepts we can update that in DB.
		- If the driver not accepted after 10 sec the TTL we implemented in distributed lock can handle the expiry of the driver lock status.
- **How we can ensure no ride requests are dropped during peak demand periods ?**
	- We can do that by introducing queues in between the API gateway and the ride matching service.
	- We can scale dynamically based on number of workers we need to process the current count of requests.
- **What happen when driver fails to respond in timely manner ?**
	- We can configure a timeout for this ride accept request to drivers.
- **How we can scale the service during surge times ?**
	- We can manage this by horizontally scaling the servers and make it available in every regions and geographical based sharding and using consistent hashing for the mapping requests.
## Reference:
![[Screenshot 2026-03-04 at 9.52.26 AM.png]]
![[Screenshot 2026-03-04 at 9.51.38 AM.png]]
![[Screenshot 2026-03-04 at 9.18.57 AM.png]]![[Screenshot 2026-03-04 at 9.27.57 AM.png]]