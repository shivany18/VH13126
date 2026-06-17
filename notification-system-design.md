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

# Stage 3: Performance Optimization for Notifications Query

## Problem

The existing query is:
SELECT * FROM notifications
WHERE studentID = 1042 AND isRead = false
ORDER BY createdAt DESC;

With the database growing to millions of records, this query becomes slow due to:

* Full table scans
* Lack of proper indexing
* Sorting large datasets


## Issues Identified

1. No index on `studentID` and `isRead`
2. Sorting (`ORDER BY createdAt DESC`) is expensive
3. Fetching all columns using `SELECT *`
4. Large dataset (5,000,000+ records)


## Optimizations

### 1. Add Indexing

Create a composite index:
CREATE INDEX idx_notifications_student_read_created
ON notifications (studentID, isRead, createdAt DESC);


This improves filtering and sorting performance.

### 2. Limit Results (Pagination)

Instead of fetching all records:

SELECT id, title, message, createdAt
FROM notifications
WHERE studentID = 1042 AND isRead = false
ORDER BY createdAt DESC
LIMIT 20 OFFSET 0;


### 3. Avoid SELECT *

Only fetch required fields to reduce load.

### 4. Database Optimization Strategies

* Use **pagination** (LIMIT, OFFSET)
* Implement **caching** (Redis) for frequent queries
* Archive old notifications to separate tables
* Use **partitioning** based on studentID or date
## Final Optimized Query

SELECT id, title, message, createdAt
FROM notifications
WHERE studentID = 1042 AND isRead = false
ORDER BY createdAt DESC
LIMIT 20;


