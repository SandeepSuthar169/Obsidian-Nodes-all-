#### How the Internet works
1. ~={4}**You request something   ->**=~ We are typing a website address such as `youtube.com`  in your browser. 
2. ~={4}**DNS finds the website  ->=~ DNS (Domain Name System) converts the website name into IP address, such as  `98.235.231.21` .
3. ~={4}**Your request travels through to network  -->**=~ your device send the request through Wi-Fi/Mobile data  **-->** route **-->** Internet Service Provider (ISP) **-->** other network. 
4. ~={4} **Data is broken into packets -->**=~
	- Information is divided into small pieces called packets.
	- Packets can travel through different routes across the internet.
5.  ~={4}**The Server responds** -->=~ The Server receives the packets and sends the requested information back. 
6. ~={4}**Your device puts everything together -->**=~ Your browser receives the packets, reassembles them, and displays the webpage. 

---
### 1.  Client vs Server
Think of a **restaurant** 🍽️:

- ~={2}**Client**=~ = customer placing an order
- ~={2} **Server**=~ = kitchen preparing the order
- ~={2}**Request**=~ = customer's order
- ~={2}**Response**=~ = food delivered to customer

Client -> is usually your browser or mobile app.
Server ->  is a computer/ program that receives requests and send response. 

```cs
          Request
Client ------------------> Server
       <------------------
          Response
```

---
### 2. HTTP vs HTTPS
HTTP -> Hyper Text Transfer Protocol
- It is set of rules that allows clients and severs to communicate. 

```http
GET /products
```

Server
```http
200 OK
Content-Type: application/json

{
	"name": "Laptop"
}
```

HTTPS
 HTTPS = HTTP + encryption using TLS

```cs
Client -------- HTTP --------> Server
http://example.com

// ---------------------------------------------
Client ===== encrypted =====> Server
https://example.com
```

---

### 3. DNS (Domain Name System)
- Computers communicate using IP addresses, but humans prefer names.
```css
Browser
   |
   | "Where is example.com?"
   ↓
 DNS
   |
   | "Here is its IP address."
   ↓
Browser
   |
   ↓
Server
```

---
### Transmission Control Protocol (TCP)
- TCP is a reliable, connection - oriented transport protocol that ensures accurate and ordered data delivery. 

```http 
File:
[Part 1][Part 2][Part 3][Part 4]
```

```http
Sender
  │
  ├── Part 1 ──────> ✅
  ├── Part 2 ──────> ✅
  ├── Part 3 ──────> ❌ lost
  ├── Part 4 ──────> ✅
  │
  │  "Part 3 didn't arrive"
  │
  └── Part 3 ──────> ✅
```

Examples:
- Web pages and many HTTP connections
- REST APIs
- File downloads
- Database connections
- SSH

---
### User Datagram Protocol (UDP
- UDP is a fast, connectionless transport protocol that sends data without reliability 
- It is efficient for applications where speed is more important than accuracy

```http
Player position:
(10,20)
(11,20)
(12,20)
(13,20)
```

Suppose `(12,20)` gets lost.

```
(10,20) → ✅
(11,20) → ✅
(12,20) → ❌
(13,20) → ✅
```
- UDP is therefore useful when **speed and freshness matter more than perfect delivery**.

Examples include:
- Online multiplayer games
- Real-time voice/video
- Some streaming systems
- DNS queries
- DHCP
----
### Cookies
- A cookie is a small piece of data that a website asks the browser to store 
#### Why are cookies useful?
They can help websites remember things such as:
- login/session identifiers
- preferences
- shopping-cart information
- certain tracking information

---
### Session 
- Server remembers the user's interaction 
---
