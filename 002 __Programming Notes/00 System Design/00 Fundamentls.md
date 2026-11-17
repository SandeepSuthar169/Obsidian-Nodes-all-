
## ~={4}What is a Server?=~

Server is a nothing but a physical machine that  we are application running on.
![[1_ZyK5a3Eaxw9vMEZrfPi2cQ.webp]]

----
---
# ~={4}Serverless vs `Serverful`=~

### 1. `Serverful` 
- in `serverful` architecture   we are responsibility for managing servers
Example: AWS EC2 instance, Docker server, physical server etc.

```css
User
  ↓
Load Balancer
  ↓
Your Server (EC2 / VM)
  ↓
Your Application
  ↓
Database
```

```css
EC2 Server
├── Node.js
├── Express
├── Your application
└── Running 24/7
```


### 1. Serverless
in serverless still have servers - but the cloud provider manages them for you 
Examples : AWS Lambda 

```css
User
  ↓
API Gateway
  ↓
Lambda Function
  ↓
Database
```

Use Case
- API requests
- Image processing
- File uploads
- Scheduled jobs
- Webhooks
- Background tasks
- Event processing

|Serverful|Serverless|
|---|---|
|You manage servers|Cloud provider manages servers|
|Server often runs continuously|Code runs when triggered|
|You manage scaling|Platform handles scaling|
|Usually pay for running infrastructure|Usually pay based on usage|
|More infrastructure control|Less infrastructure management|
|Long-running applications are easy|Short/event-driven workloads are convenient|

---
---
# ~={4}Horizontal vs Vertical Scaling=~
- Vertical scaling = make one machine bigger  
- Horizontal scaling = add more machines

### ~={44}Vertical Scaling ( scaling up ).=~
- take existing server and give it more resources

```css
Before:

      Server
   4 CPU
   8 GB RAM

          ↓ SCALE UP

      Server
   16 CPU
   64 GB RAM
```

Advantages
- It's relatively simple because we are application can continue running on one machine.

Disadvantages
- If that one server fails we are application can go down. 

#### Database use
-  PostgreSQL
- MySQL
- SQL Server
- Oracle

--- 
### ~={44}Horizontal Scaling ( scaling out ).=~
- you add more servers

```css
             Load Balancer
             /     |      \
            ↓      ↓       ↓
         Server  Server  Server
           1       2       3
```

#### ~={2} Horizontal Scaling server problem =~
- Suppose a user logs in, 
- Their session is stored on Server 1, 
- Then their next request goes to Server 2
- Server 2 doesn't have the session if the session exists only in Server 1's memory.

```css
             Load Balancer
            /      |      \
           ↓       ↓       ↓
       Server 1 Server 2 Server 3
            \      |      /
             ↓     ↓     ↓
              Redis / Database
```

#### Database use 
- MongoDB
- Cassandra
- DynamoDB

| Vertical Scaling                      | Horizontal Scaling                   |
| ------------------------------------- | ------------------------------------ |
| Add more CPU/RAM to one server        | Add more servers                     |
| Scale **up**                          | Scale **out**                        |
| Simpler architecture                  | More complex architecture            |
| Limited by machine capacity           | Can potentially add many machines    |
| Single machine can be a failure point | Better redundancy                    |
| Usually easier for stateful apps      | Often better for distributed systems |

---
### ~={44}Auto Scaling =~
- Horizontal scaling becomes especially powerful when combined with auto scaling.

```css
Traffic increases
       ↓
CPU > 70%
       ↓
Add Server 4
       ↓
Traffic increases more
       ↓
Add Server 5
```

---
---
## ~={4}Latency and Throughput=~

Latency : is the time taken for a single request to travel from the client to the server and back 
- It is measured in milliseconds ( ms ). 
- Latency measures the time to process a single request.
![[e2a23b_28272e91780d415693775fc89be28669~mv2-2683238266.webp]]

#### RTT ( Round Trip Time )?

- RTT is the time in milliseconds it takes for a data packet to travel from a source to a destination and back again. 

![[RTT-660.png]]

## ~={4}Throughput=~
- Throughput is the number of requests or units of works the system can handle per secund.
- It is typically measured in requests per second  ( RPS ) or transactions per second ( TPS ).
- Every server has a limit. Which means it can handle X number of request per section and It may be go down server.
- Throughput measures how many requests can be handled concurrently.
#### High Throughput
- The system can process many requests at the same time.
#### Low Throughput
- The system struggles to process many requests concurrently. 
----
---
## ~={4}Back-of-the-envelope Estimation=~
- in Horizontal scaling we see that we need to more servers to handle the loads how much number of storage, and servers needs. 

##### 1. Load Estimation
- Load estimation is daily active users mean calculate the number of reads and the number of writes.
~={1}Example=~
- Twitter has 100 million daily active users and user posts 10 tweets per day.
- 1 user posts 10 tweets per day.
- Number of tweets in on day
	- 100 million * 10 tweets  = 1 billion tweets per day
	- Examples 1 user reads 100 tweets per day.
	- Number of reads in one day
		- 100  million * 100 tweets  => 10 billions reads per day
##### 2. Storage Estimation
- 
##### 3. Resource Estimation


------
----
# CAP Theorem
- CAP Theorem is a trade-off while designing and system.

- Distributed System means data is stored in multiples servers instead of one,
	-  With multiple servers we can spread the workload & handle more requests simultaneously, improving overall performance.

- C : Consistency
- A: Availability 
- P : Partition Tolerance 

One individual server that is part of the overall distributed system is called `Node (Database)`
#### C: Consistency
- Evert user gets the same, latest data. 
- Example:
	- You change your bank balance to ₹500.
	- Immediately after, you ask another server for your balance.
	- It must also say **₹500**.
```
             ┌─────────────┐
User ───────►│  Server 1   │──┐
             └─────────────┘  │
                              ▼
                         ┌─────────┐
                         │Database │
                         └─────────┘
                              ▲
             ┌─────────────┐  │
User ───────►│  Server 2   │──┘
             └─────────────┘
```

####  A: Availability
- Every request gets a response, even if something is ging wrong:
- Example:
	- Server A cannot communicate with Server A.
	- A user asks Server B for their balance. 
	- Server B still give some response instead of saying "I have not answer".

```
             ┌─────────────┐
User ───────►│  Server 1   │
             └─────────────┘  
                    │
                    │
                    ▼
                    ❌         
                    ▲
                    │
                    │         
             ┌─────────────┐  
User ───────►│  Server 2   │
             └─────────────┘
```
