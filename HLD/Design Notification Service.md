#Round2 #Freshworks 
## System Overview:
- A system that can be used to deliver timely, relevant messages to users through various channels such as 
	- Email
	- SMS
	- in-app messages.
## User: 
	- Person who configures the messages that needs to be sent to the end customer.
	- Person who receives the messages from the server.
## Functional Requirements:
- The admin can able send messages to customers through multiple channels like
	- SMS
	- Email
	- Push notifications.
- The admin should be able to do CRUD operations in the message template they have added.
- The admin can able to configure
	- Immediate messages
	- Scheduled messages
- The customer can able to configure message preferences and its type.
- The admin should able to see the delivery status of the messages.

## Non Functional Requirements:
- Scale:
	- 1M request per second.
- CAP:
	- By default we have high availability.
	- If we configure a notification template and read it immediately then the stale template can come which is fine now.
	- Availability >> consistency
- Latency: 
	- Bank or OTP: immediate delivery low latency
	- Promotional Notification: 10 seconds delay is accepted.

## Core Entities:
~~~
Client
User
UserPreference
NotificationTemplates
NotificationTrack
~~~
## APIs:
~~~
POST /api/v1/template
{
	template: 	
}

GET /api/v1/template/{templateId}

POST /api/v1/notification
{
	templateId: 
	receipientId: 
	variables:
	channel: 
	priority:
	scheduledAt: 
}

GET /api/v1/notification/{notificationId}

POST /api/v1/preference:
{
	clientId:
	userId:
	preferences:{
		email"
		sms:
		inApp
	}
}
~~~

## High level design:
![[Screenshot 2026-03-05 at 11.18.11 AM.png]]

## Deep dives:
![[Screenshot 2026-03-05 at 12.45.30 PM.png]]