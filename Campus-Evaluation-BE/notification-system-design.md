# Notification System Design
#Stage 1: Notification System Design 

##Core Actions
-Create notifications
-Get notifications
-Mark as Read
-Delete Notifications

#REST APIS

POST /notifications
GET /notifications/:userId
PUT /notifications/:id/read
DELETE /notifications/:id

#Sample request
{
    "userId":"123",
    "title":"New message",
    "message":"You have a new message"
}

#Sample Response

{
    "id":"n1",
    "userId":"123",
    "title":"New Message",
    "isRead" : false
}

##Headers
Content-Type : application/jso
Authorization: Bearer token

#Real-Time Notifications
Use WebSockets to send notifications instantly to users when they are online.

#Stage 2

###Database Choice
I suggest using MySQL 

###Reason 
-Structured data (fixed fields like userId, message)
-Easy quering using SQL
- Supports indexing for perfomance optimization

###Table Schema

-id (INT, PRIMARY KEY, AUTO_INCREMENT)
-studentID(INT)
-title(VARCHAR)
-message(TEXT)
-isRead(BOOLEAN)
-createdAt(TIMESTAMP)

### Problem with Large Data

-Slow query perfomance
-Large table size
-Increased respoonse time

## Solutions

-Add indexes on studentId and isRead
-Use pagination (LIMT)
-Archive old notifications
-Use caching (Redis)

## Sample queries

get notifications 
SELECT * FROM notifactions WHERE studentID = 1042;

get unread:
SELECT * FROM notifications WHERE studentId =1042 AND isRead = false;

Mark as read:
UPDATE notifications SET isRead = true WHERE id=1;