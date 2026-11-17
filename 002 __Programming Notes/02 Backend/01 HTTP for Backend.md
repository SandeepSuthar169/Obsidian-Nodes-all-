# ~={orange}HTTP=~ 
- The medium through it which are browser talks to a server either to send data or receive  data from the internet. 

- Which client and server use to communicate with each other 
	- HTTP
	- Web Socket 
	- Server Actions/ RPC

- The heart of HTTP protocol
	- State Less
	- Client - Server model

----
#### ~={purple} 1. State less =~
It has not  memory of past interaction between client and server 
- HTTP request carries all the necessary information of the server to process
- such as Header 
- URL 
- Methos 
- After server responds client forgets about the request
- Client send another request the server treats it as a new request and send new response.

#### ~={purple}2. Client - Server model =~
In this architecture of how communication happens on the web.
- Client - who requests something 
- Server - who responds with the requested resource 

```
CLIENT                          SERVER
    |                               |
    |  ---- HTTP Request  ------->  |
    |  GET /api/users               |  (processes request)
    |                               |
    |  <--- HTTP Response --------- |
    |  200 OK + JSON data           |
    |                               |
```

---
### ~={purple}TCP — Transmission Control Protocol=~
TPC is reliable, ordered, connection-oriented, it acts like a phone call like it connection first, then talk, and both sides confirm it 

When to use TCP
- using Website
- Email and file transfer
- Anything where every byte must arrive intact and in order -> web pages, JSON APIs, file download, database connections.

### ~={purple}UDP — User Datagram Protocol=~
UDP is unreliable, unordered, connectionless. it acts like sending postcards and you hope they arrive it, No handshake, no acknowledgments, no retransmission.

When to use UDP
- Video/audio streaming, Online games

---

### ~={purple}The 7 Layers of the OSL Model=~

```   
LAYER 7  APPLICATION     ← what the user interacts with (HTTP, DNS, FTP)
   LAYER 6  PRESENTATION    ← format/encrypt/compress data (TLS, JPEG, UTF-8)
   LAYER 5  SESSION         ← manage conversations (session setup/teardown)
   LAYER 4  TRANSPORT       ← end-to-end delivery, ports (TCP, UDP)
   LAYER 3  NETWORK         ← routing across networks, IP addresses (IP, ICMP)
   LAYER 2  DATA LINK       ← hop-to-hop delivery on same network, MAC (Ethernet, Wi-Fi)
   LAYER 1  PHYSICAL        ← actual bits on the wire/air (cables, radio, voltages)
```

---
### REQUEST 
```ts
PUT /api/users/12345 HTTP/1.1
Host: example.com
//                             Headers 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.82 Safari/537.36
Content-Type: application/json
Content-Length: 123
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Accept: application/json
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Referer: https://example.com/dashboard
Cookie: sessionId=abc123xyz456; lang=en-US

// a blank line here means, everything has been sent

{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "age": 30
}
```
### RESPONSE 
```TS
HTTP/1.1 200 OK
Date: Fri, 20 Sep 2024 12:00:00 GMT
//                             Headers 
Content-Type: application/json
Content-Length: 85
Server: Apache/2.4.41 (Ubuntu)
Cache-Control: no-store
X-Request-ID: abcdef123456
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
Set-Cookie: sessionId=abc123xyz456; Path=/; Secure; HttpOnly
Vary: Accept-Encoding
Connection: keep-alive

// a blank line here means, everything has been sent

{
  "message": "User updated successfully",
  "userId": 12345,
  "status": "success"
}
```

---
## ~={orange} Headers Categories =~
### ~={purple} 1. Request Headers=~
This is send by the client to the server 
- User-Agent -> What kind of Client it is like Browser, Postman, kind of server, Mobile App.
- Authorization -> Which is send credentials like Barer Token to the server to identify the user
- Cookie -> 
- Accept -> Which provides information like what kind of content like JSON, Text, HTML files 

### ~={purple}2. General Headers=~
- Data -> 
- Cache-Control -> 
- Connection -> 
### ~={purple}3. Representation Headers=~
The primary deal with the representation of the resource being transmitted  whether it could be a request body or a of the resource being transmitted 

Representation Headers provides information about the body of the message.

- Content-Type  -> It the describes the media type of the request will JSON, HTML.
- Content-Length  -> it the describes the size of the resource in Bites 
- Content-Encoding -> it the describe the any encoding like G Flare or D Flare 
- ETag -> Mostly use of Caching 

### ~={purple}4. Security Headers=~
Which are used to enhance the security of the request and response by Security Headers and controlling behaviour like content loading,  Cookies and Encryption HSTS 

- Strick-Transport-Security (HSTS) -> HSTS ensures that the client only communicated with the server over HTTPs.
- Content-Security-Policy (CSP) -> The preventing protocol downgrade arrack sources from which content like JavaScript or CSS and images can be loaded helping prevent cross site scripting attacks. 
- X-Frame-Options  -> The preventing the web page from being embedded in I frame mitigation click jacking attack.
- X-Content Type-Options  -> The ensures that the browser does not try to guess the MIME type of the content preventing 
- Set-Cookie -> 

------

## ~={blue}Extensibility =~
Extensibility -> HTTP is highly extensible because headers can be easily added or customized without altering the underling protocol
## ~={blue}Remote Control =~
HTTP headers work kind of a remote control on the server side they allow the client to send instructions or preferences to the server influencing how the server response or processes requests 

----

| Method | Action       | Body? | Idempotent  | Safe | Typical Status | Definition                                                       |
| ------ | ------------ | ----- | ----------- | ---- | -------------- | ---------------------------------------------------------------- |
| GET    | Read         | No    | ✅ Yes       | ✅    | 200            | Fetch/read a resource without modifying it.                      |
| POST   | Create       | Yes   | ❌ No        | ❌    | 201            | Submit data to create a new resource or trigger processing.      |
| PUT    | Replace      | Yes   | ✅ Yes       | ❌    | 200 /  204     | Fully update/replace an existing resource with the provided data |
| PATCH  | Partial edit | Yes   | ✅ (usually) | ❌    | 200 / 204      | Apply a **partial** modification to a resource.                  |
| DELETE | Remove       | Rare  | ✅ Yes       | ❌    | 204 / 200      | Delete a specified resource.                                     |

IMP    

| Request                  | Payload | Payload reduction |
| ------------------------ | ------- | ----------------- |
| PUT — full resource      | 100 KB  | 0%                |
| PATCH — 10 KB of changes | 10 KB   | 90%               |

---

## ~={blue}Idempotent=~
HTTP methos can be called multiple times and we can expect the same kind of results. 
Example -> ~={1}`GET, PUT, DELETE`=~.

## ~={blue}Non-Idempotent=~
Which HTTP methos can be called it and we can expect the deferent kind of results or data.
Example -> ~={1}`POST, PATCH`=~ .

-----

## ~={blue}HTTP OPTIONS =~
HTTP Options asks a server what communication options or actions  ( such as GET, POST, PUT, DELETE ) are allowed for a specific URL or resource. 

---

## ~={blue}CORS - Same Origin Policy (SOP)=~
It is a browser security mechanism that uses HTTP headers to let a server allow to block web requests from a different domain on the internet. 

| URL A                     | URL B                      | Same Origin? | Why                        |
| ------------------------- | -------------------------- | ------------ | -------------------------- |
| `https://example.com/app` | `https://example.com/api`  | ✅ Yes        | Same protocol/host/port    |
| `https://example.com`     | `http://example.com`       | ❌ No         | Different protocol         |
| `https://example.com`     | `https://api.example.com`  | ❌ No         | Different host (subdomain) |
| `https://example.com`     | `https://example.com:8080` | ❌ No         | Different port             |

---
## ~={blue}Preflight Request=~
- The method is not GET, POST, or HEAD ( ex.  PUT, DELETE )
				    	or 

 - The request includes non-simple headers ( Like Authorization, X-Custom-Headers )
				    	or 

- The request has a Content-Type other than application/ x-www-form-urlencoded, multipart/form-data or text/plain.


Browsers automatically send `OPTIONS` request ahead of cross-origin `post`,  `put`, `DELETE` calls to verify security permissions. 

##### (browser → server)
```ts
OPTIONS /api/users/42 HTTP/1.1
Host: api.example.com
Origin: https://app.example.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: content-type, authorization
User-Agent: Mozilla/5.0 ...
Accept: */*
```
#### server → browser)
```ts
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 86400
Vary: Origin
```

----
## ~={blue}Simple Request=~

Methos -> GET, HEAD, POST
Headers -> Only CORS- safelisted  headers
Content-Type if(post) -> `text/plain`, `application/x-www-form-urlencoded`, `multipart/form-data`

|Aspect|Simple Request|Preflighted Request|
|---|---|---|
|Round trips|1|2 (OPTIONS + real)|
|Methods|GET, HEAD, POST|Any (PUT, DELETE, PATCH…)|
|Content-Type|form/text only|any (e.g. `application/json`)|
|Custom headers|❌ Not allowed|✅ Allowed|
|`Authorization` header|❌ Triggers preflight|✅ Allowed|
|Has `OPTIONS` call|No|Yes|
|Latency|Lower|Higher (unless `Max-Age` caches it)|

----

## ~={cyan}HTTP Status Code=~

##### ~={yellow}1. 1xx — Informational=~ 

|Code|Name|When to use|
|---|---|---|
|**100**|Continue|Server received headers, client should send body (used with `Expect: 100-continue` for large uploads)|
|**101**|Switching Protocols|Upgrading connection — e.g., WebSocket handshake (`Upgrade: websocket`)|
|**103**|Early Hints|Send `Link` preload hints before final response (perf optimization)|

##### ~={yellow}2. 2xx — Success=~

| Code    | Name            | When to use                                                                         |
| ------- | --------------- | ----------------------------------------------------------------------------------- |
| **200** | OK              | Generic success — GET, PUT, PATCH returning a body                                  |
| **201** | Created         | Resource was created — **POST** (return `Location` header pointing to new resource) |
| **202** | Accepted        | Request accepted for async processing (job queued, not done yet)                    |
| **204** | No Content      | Success but nothing to return — **DELETE**, PUT/PATCH with no body                  |
| **206** | Partial Content | Range request (video streaming, resumable downloads)                                |

#### ~={yellow}3xx — Redirection=~

| Code    | Name               | When to use                                                                      |
| ------- | ------------------ | -------------------------------------------------------------------------------- |
| **301** | Moved Permanently  | URL changed forever — update bookmarks/SEO. Browser caches it                    |
| **302** | Found (Temporary)  | Temporary redirect — **most common** for login redirects, form POST→GET          |
| **303** | See Other          | Redirect after POST → force client to `GET` the result (PRG pattern)             |
| **304** | Not Modified       | Cached resource is still valid (used with `If-None-Match` / `If-Modified-Since`) |
| **307** | Temporary Redirect | Like 302 but **preserves method** (POST stays POST)                              |
| **308** | Permanent Redirect | Like 301 but **preserves method**                                                |

**301 vs 302 vs 307 vs 308:**
- **301/308** = permanent (cacheable, SEO-friendly)
- **302/307** = temporary
- **302/303** may change POST → GET; **307/308** preserve the method

#### ~={yellow}4xx — Client Errors=~

|Code|Name|When to use|
|---|---|---|
|**400**|Bad Request|Malformed syntax, invalid JSON, missing required field|
|**401**|Unauthorized|**Not authenticated** — no/invalid credentials. Send `WWW-Authenticate` header|
|**403**|Forbidden|Authenticated but **not allowed** (no permission)|
|**404**|Not Found|Resource doesn't exist (or hidden for security)|
|**405**|Method Not Allowed|Endpoint exists but wrong verb (e.g., `DELETE` on read-only). Send `Allow` header|
|**406**|Not Acceptable|Can't satisfy `Accept` header (e.g., client wants XML, only JSON available)|
|**408**|Request Timeout|Client took too long to send request|
|**409**|Conflict|State conflict — duplicate email, version mismatch, concurrent edit|
|**410**|Gone|Resource existed but permanently deleted (stronger than 404)|
|**413**|Payload Too Large|Upload exceeds limit|
|**415**|Unsupported Media Type|Wrong `Content-Type` (e.g., sending XML to JSON-only API)|
|**422**|Unprocessable Entity|Syntax OK but semantic errors — **validation failures** (common in REST APIs)|
|**429**|Too Many Requests|Rate limited. Send `Retry-After` header|
#### ~={yellow}5xx — Server Errors=~

| Code    | Name                  | When to use                                                   |
| ------- | --------------------- | ------------------------------------------------------------- |
| **500** | Internal Server Error | Unhandled exception — generic "something broke"               |
| **501** | Not Implemented       | Server doesn't support the method at all                      |
| **502** | Bad Gateway           | Proxy/gateway got invalid response from upstream              |
| **503** | Service Unavailable   | Server overloaded or down for maintenance. Send `Retry-After` |
| **504** | Gateway Timeout       | Proxy/gateway timed out waiting for upstream                  |

## ~={blue}Mapping Status Codes to CRUD Operations=~

|Operation|Success|Common Errors|
|---|---|---|
|`GET /users`|200|401, 403|
|`GET /users/42`|200|401, 403, 404|
|`POST /users`|**201** (+ `Location`)|400, 409, 422|
|`PUT /users/42`|200 or 204|400, 404, 409, 422|
|`PATCH /users/42`|200 or 204|400, 404, 422|
|`DELETE /users/42`|**204**|401, 403, 404|

---

## ~={orange}HTTP Caching =~
HTTP caching -> storing a copy of HTTP response so that the client or browser. intermediary can reuse it instead of requesting the same data from the server every time.

HTTP caching headers 

~={44}Cache-control =>=~ Defines whether and how long a response can be cached.
~={44}ETag => =~Identify a particular version of a response.
~={44}If-None-Match =>=~ Lets a client ask whether its cached ETag is still current.
~={44}Last-Modified =>=~ Indicates when the resource was last changed
~={44}If-Modified-Since =>=~ Lets a client validate a cached response using a timestamps 

```ts
HTTP/1.1 200 OK
Cache-Control: private, no-cache
ETag: W/"abc123"
Content-Type: application/json
```

---

```js
import express, { type Request, type Response } from "express"
import { todos, type Todo } from "../db/todo"

export const getTodo = (req: Request, res: Response) => {
    try {

        // 1. Request Headers

        console.log("All Headers:",req.headers);
        console.log("=======================================");
        console.log("User-Agent           :", req.headers["user-agent"]);         
        console.log("Authorization        :", req.headers["authorization"]);     
        console.log("Cookie               :", req.headers["cookie"]);            
        console.log("Host                 :", req.headers["host"]);             
        console.log("accept-encoding      :", req.headers["accept-encoding"]);            
       
        console.log("=======================================");

        // 2. General Headers

        console.log("=== REQUEST general headers ===");

        res.set({
            Date: new Date().toUTCString(),
            "Cache-Control": "no-store",
            Connection: "keep-alive"
        })
        
        console.log("Date              :", req.headers["date"]);                    // usually undefined
        console.log("Cache-Control     :", req.headers["cache-control"]);           // usually undefined
        console.log( "Connection       :",req.headers["connection"]);

        
        console.log("=== RESPONSE general headers ===");
        console.log("Date         :", res.get("Date"));
        console.log("Cache-Control:", res.get("Cache-Control"));
        console.log("Connection   :", res.get("Connection"));    

        
        console.log("======================================");

        // 3. Representation Headers

        console.log("=== Representation headers ===");
        res.on("finish", () => {
            console.log("=== RESPONSE representation headers ===");
            console.log("Content-Type    :", res.get("Content-Type"));
            console.log("Content-Length  :", res.get("Content-Length"));
            console.log("Content-Encoding:", res.get("Content-Encoding"));
            console.log("ETag            :", res.get("ETag"));
        });
        
          // Helmet already set these on `res` before we got here
        console.log("=== RESPONSE security headers ===");
        console.log("Strict-Transport-Security:", res.get("Strict-Transport-Security"));
        console.log("Content-Security-Policy  :", res.get("Content-Security-Policy"));
        console.log("X-Frame-Options          :", res.get("X-Frame-Options"));
        console.log("X-Content-Type-Options   :", res.get("X-Content-Type-Options"));

        res.json({
            data: todos,
            success: true
        })
        
    } catch (error) {
        console.error(error);
    }
}

export const createTodo = (req: Request, res: Response) => {

  //1. Representation headers 
  console.log("=== REQUEST representation headers ===");
  console.log("Content-Type    :", req.headers["content-type"]);
  console.log("Content-Length  :", req.headers["content-length"]);
  console.log("Content-Encoding:", req.headers["content-encoding"]); // usually undefined

  const { title } = req.body;
  if (!title) {
    return res.status(400).json({ success: false, message: "title is required" });
  }

  const todo = { id: Date.now(), title, done: false };
  todos.push(todo);

  res.set("Cache-Control", "no-store");
  res.status(201).json({ success: true, data: todo });
};

export const login = (req: Request, res: Response) => {
  res.cookie("sessionId", "abc123", {
    httpOnly: true,      // JavaScript (document.cookie) cannot read it
    secure: true,        // only sent over HTTPS (localhost is an exception in browsers)
    sameSite: "strict",  // not sent on cross-site requests (CSRF protection)
    maxAge: 60 * 60 * 1000, // 1 hour
  });

  console.log("=== RESPONSE Set-Cookie ===");
  console.log("Set-Cookie:", res.get("Set-Cookie")); // this is an array

  res.json({ success: true, message: "Logged in" });
};
```