## ~={red}What Are Background Jobs?=~
- A background job or task is any piece of code that runs outside the main request-response lifecycle. 
- A background tasks handle non-critical operation that can be processed asynchronously in a separate process. 
	- ~={orange}e.g. =~
		1. User submits email, name, password
		2. Backend validates the data
		3. Backend needs to send verification email
		4. process Image and Videos.
		5. Generating Reports  
			1.  In Corporate Job sending monthly/weekly reports (PDF, Email).
		6. Sending Push notification.

Bull-MQ

![[Screenshot From 2026-11-02 05-20-41.png]]

---

Visibility Timeout
#### ~={blue}1. Producer (Job Creator)=~
-  The Producer is the part of Backend that creates and sends a task to the queue.
	- API server.
	- It collects required data. 
	- it serializes the data ( often JSON )
~={purple}EXAMPLE=~ : 
	- User signs up -> API validates input -> instead of sending email immediately 
	 ~={pink}sendVerificationEmail()=~
	  
#### ~={blue}2. Queue (Broker)=~
- The queue / Broker is the storage layer that temporarily holds tasks until a worker is ready to process them. 
- **Redis** (used by many Node.js queue libraries)
#### ~={blue}3. Consumer (Worker)=~
- A separate process that picks up tasks from the queue and executes them. 
	-  Run the actual business logic (e.g., call an email provider API).

---

## ~={red}Core Concepts of a Task Queue=~
- A task queue is the system that manages and distributes background jobs or tasks. 

![[Screenshot From 2026-11-02 11-17-17.png]]


### ~={orange}Types of Queues Architecture =~
~={blue}1. Push Based Mechanism=~
- The queue automatically send messages to consumers whenever messages are available. 
- Ex. RabbitMQ push consumers, Redis Pub/Sub

~={yellow}Advantages=~:
- Low message delivery latency
- Consumers don't need to continuously poll.
- Efficient for event-driven system

~={yellow}Disadvantages:=~
- Consumer can be overwhelmed if messages arrives faster than  they can process them

~={blue}2. Pull Based Mechanism =~
- The consumer requests messages from the queue whenever it it ready to process them.
- Ex. Amazon SQS polling, Apache Kafka consumers. 

~={yellow}Advantages=~:
 - Consumers control their processing speed.
 - Consumers can request batches of messages

~={yellow}Disadvantages:=~
- Frequent polling can waste resources.
- Messages might experience additional delivery latency.

---
### ~={orange}Starvation in Queues=~
- Starvation in Queues is a situation in which a message or task waits indefinitely in queue because other messages or tasks continuously get priority over it.

~={red}1. Real-world example=~
~={red}   Imagine a restaurant with two types of customers:=~
~={red}- VIP customers (high priority)=~
~={red}- Regular customers (low priority)=~
~={red}The restaurant always serves VIP customers first.=~

~={red}If VIP customers keep arriving continuously, regular customers might never get served.=~

~={red}This is called starvation.=~

---

### RabbitMQ Push based Message Delivery Mechanism
- This RabbitMQ sends messages to consumers automatically when messages are available and the consumers have capacity to receive them.

![[k9WI6WRMgQ79pyG9oxbDVP3DYTTz1qrYnlUJkcPrf1HtE8n67rAs7BYcz39VtFUz0uQLe_b5PWMaqIA3AB5mitz0HsMupfpyh7ntEbTH6CAW5XKAQS5ob1OMDNSHhzqdUqdUbUox0DmkJnGSO5LHXgnk5OuuGXM4B570ici6BkiYa4PpoaEsrRSQv5Aht5LJ.jpeg]]

#### ~={red}Heartbeat Mechanism in RabbitMQ=~
- It is mechanism used to detect whether a connection between RabbitMQ and a consumer is still alive.

---

### ~={orange}Type of Tasks=~
1. One-off tasks -> Single tasks
Examples
- Sending verification email after user registration.
- Sending welcome email.
- Sending password reset email.
- Sending an order confirmation notification.
- Sending a payment success notification.
Key point: One event triggers one task execution. Retries may happen if processing fails.


2. Recurring Tasks (Scheduled Tasks) ->  Executed periodically in specific time
Example
- Sending daily reports.
- Generating monthly financial reports.
- Sending annual subscription reminders.
- Cleaning expired sessions.
- Removing old temporary files.
- Database maintenance jobs.
- It is Cleanup or Maintenance Job

How it works:
1. Scheduler checks the configured schedule.
2. At the scheduled time, it creates a task.
3. Scheduler publishes the task to RabbitMQ.
4. Worker receives and processes it.
5. The process repeats according to the schedule

Example:
Task             	Execution
Daily report  -> 	Every day at 12 AM
Monthly report -> 	First day of every month
Cleanup job	Every ->  6 hours
Annual report -> 	Once every year

 6. Chained Tasks -> Parent child relationship with Tasks
 - Ex -> 
```
Parent Task: Process Order
 
Child Task 1: Validate Order

Child Task 2: Process Payment

Child Task 3: Update Inventory

Child Task 4: Create Shipment

Child Task 5: Send Confirmation Email
```

![[pirlr5jN6KnuPfp3VU9EZuvHvVTkEO7C10G7ZxEO7LykQw8Lf4jRetWh10qaa73Gyis1-WDPfD4dqFgS-butIFvGfyrE77y3HckYThwljTGOY4vMyVMjQY3NA_MKkLfxRKq9CyNeSheSv8eP76XNpcW4SgnsLMW0y2YMUa-H_1M2hvQ-ZEQk_huNnkiHwFRt.jpeg]]

6. Batch Tasks  -> Tasks processes a large collection records together rather than processing each record as an independent real time event.

Examples:
- Sending promotional emails to 100,000 users.
- Processing 50,000 payment records.
- Generating reports for 1 million customers.
- Importing large CSV files.
- Updating product prices in bulk.
![[WpzGLCRPbRHztcmYMZFyWKPLq4cstKB0yxtvkNevf6WFl8K77k6jc5QPcOoiG-vbCFgjD2QDU7B8ky0hy_3ZVoQiilBJ8dtqAmX61G0DyZlJLmS4NkrUmyomBeYrNlgbL9SQwJ6PHKk39DRqDpQUc_uxaMVcM7iAnbrmGGhllfo.jpeg]]

Important: Track chunk completion, handle retries, and make processing idempotent to avoid duplicate operations.


---

## Designing a reliable message queue system
~={blue}1. Idempotency=~
- Processing the same message multiple times should produce the same final result as processing it once.

Example: Payment Processing
Suppose a customer pays ₹1,000.
1. Payment Worker receives the message.
2. Worker successfully processes the payment.
3. Worker crashes before sending ACK.
4. RabbitMQ doesn't receive the ACK.
5. RabbitMQ delivers the same message again.
6. Worker processes it again.

How to implement it?
Assign a unique ID to every message.
```json
{
  "messageId": "MSG_101",
  "paymentId": "PAY_101",
  "amount": 1000
}
```


Maintain a database record of processed message IDs.
```PostgreSQL
CREATE TABLE processed_messages (
    message_id VARCHAR(100) PRIMARY KEY,
    processed_at TIMESTAMP
);
```

Before processing:
1. Check whether the message ID already exists.
2. If it exists, don't repeat the business operation.
3. Otherwise, process the message and record its ID.
Important: The business operation and recording the processed ID must be atomic, typically using a database transaction. For external payment APIs, use their idempotency-key support where available.

----
~={blue}2. Error Handling=~
- Error handling defines what happens when a worker fails to process a message.
- Not every failure should be handled in the same way.


| Error     | Example               | Handling                |
| --------- | --------------------- | ----------------------- |
| Temporary | Database unavailable  | Retry                   |
| Network   | API timeout           | Retry                   |
| Permanent | Invalid email address | Reject or DLQ           |
| Business  | Insufficient balance  | Handle business failure |
Example retry policy
- First failure → Retry after 5 seconds.
- Second failure → Retry after 30 seconds.
- Third failure → Retry after 2 minutes.
- Maximum attempts reached → Move to DLQ.

---
~={blue}3. Monitoring=~
Monitoring means continuously tracking the health and performance of your queue system.

Monitoring tools
- RabbitMQ Management UI
- Prometheus
- Grafana
- OpenTelemetry

---
~={blue}4. Scaling=~
Scaling means increasing the system's capacity to process more messages.

Scaling strategies
1. Horizontal scaling: Add more worker instances.
2. Vertical scaling: Increase CPU and memory of existing workers.
3. Auto-scaling: Automatically add or remove workers based on queue depth or processing latency.
Important: Adding workers doesn't always increase throughput. Your database, external APIs, or other downstream services might become bottlenecks.

---
~={blue}5. Ordering=~
Ordering means ensuring messages are processed in the required sequence.

A customer performs three operations:
1. Create order.
2. Process payment.
3. Ship order.

---
~={blue}6. Rate Limiting=~
Rate limiting controls how many messages can be processed or how many requests can be sent to an external service within a specific time.

---

Best Practices

- Keep tasks small and focused
- Avoid long running tasks
- User proper error handling and logging
- Monitory queue length and worker health


# End

