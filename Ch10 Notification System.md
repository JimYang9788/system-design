## Design Notification System
Three types of notification system exist:
* SMS message 
* mobile push notification
* Email

#### Questions to ask 
* Is it real time? 
* What triggers the notification? (Server / Client)
* Can user opt out 
* How many notifications are sent out each day 

### Implementation
APNS -> IOS 
FCM -> Android 
SMS Service -> SMS 
Email Service -> Email

### Reliability 
How to prevent data loss? Notification can usually be delayed or re-oredered, but never lost. We can persists notification data in a database and implements a retry mechanism. A notification log databse is included for data persistence. 