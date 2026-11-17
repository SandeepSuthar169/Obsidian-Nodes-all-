## ~={red}Part 1=~
---
~={orange}Middleware=~ : 
- Middleware is a function thar run between the moment your server receives a request and the moment specific endpoint code (controller) handles it. 
	-  `req` (the request object)
	- `res` (the response object)
	- `next()` (a call-back to pass control to the next middleware)
```js
	(err, req, res, next)
```

```js
import jwt from "jsonwebtoken"

export const isLoggedIn = async (req, res, next) => {
	console.log(req.cookies);
    let token = req.cookies?.token 

	console.log("Token Found: ", token ? "YES": "NO");
        
    if(!token){
        console.log("NO token");
        return res.status(401).json({
            success: false,
            message: "Authentication failed"
        })
    }

    const decoded  = jwt.verify(token, process.env.JWT_SECRET)
    console.log("decoded data", decoded);
    req.user = decoded
        
    next()
} 
```

```js
import mongoose from "mongoose";
import { jwt } from "jsonwebtoken";
import { User } from "../models/user.models.js"
import { ApiError} from "../utils/api-error.js"
import { asyncHandler } from "../utils/async-handler.js"
import { ProjectMember } from "../models/projectmember.models.js";

export const verityJWT = asyncHandler(async (req, res, next) => {
    const token = req.cookies?.accessToken || req.header("Authorization")?.replace("Bearer ", "")

    if(!token){
        throw new ApiError(401, "Unauthorized request");
    }

    const decodedToken = jwt.verify(token,
        process.env.ACCESS_TOKEN_SECRET)
        const user = await User.findById(decodedToken?._id).select("-password -refreshToken -emailVerificationToken -emailVerificationExpiry")

    if(!user){
        throw new ApiError(401, "Invalid access token")
    }    
    
    req.user = user
    next()
})

```

---
~={orange}**CORS (Cross-Origin Resource Sharing)**:=~
- CORS is a security rule in browsers that controls which websites are allowed to request resources from another website  (different origin).
- Browsers implement the **same-origin policy** that blocks requests from different origins (domain, protocol, or port).

```js
import express from "exress"
import cors from "cors"
import app from "express"

// Enable all CORS requests
app.use(cors({
  origin: 'https://example.com', // Allow only this domain
  methods: ['GET', 'POST'], // Allowed methods
  allowedHeaders: ['Content-Type', 'Authorization'], // Allowed headers
  credentials: true, // Allow cookies
  maxAge: 86400 // Cache preflight request for 1 day
}));

app.get('/api/data', (req, res) => {
  res.json({ message: 'CORS-enabled route' });
});

app.listen(3000);
```

---
### ~={orange}Rate Limiting=~
- Rate limiting is a technique used to limit how many requests a user (or IP address ) if a client is sending too many requests too quickly it can return a `429 Too Many Requests`
-  Protects against **brute-force attacks**
```js
import express from "express"
import rateLimit from "express-rate-limit"

const app = express();

// Create limiter
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per window
  message: "Too many requests, please try again later."
});

// Apply to all routes
app.use(limiter);

app.get('/', (req, res) => {
  res.send("Welcome!");
});

app.listen(3000);
```
---
## ~={red}Part 2=~

### ~={blue}The Controller (or Handler)=~
- The controller is the entry point for your specific API route ( e.g. ~={green}POST  /api/book=~ )
- Its role is to manage the HTTP-specific of the request and response. 
#### ~={blue}Parts of Controller=~
1. ~={purple}**Extract Data** =~: -> It pulls data from the request body for POST/PUT/PATCH, query parameters 
	- GET requests: Extract query parameters (e.g., `/products? category=electronics&limit=10` → extract `category` and `limit`)
	- POST requests: Extract quest body 

2. ~={purple}**Deserialization** ( Converting JSON to Language Objects)=~  : -> The request body is usually in JSON formant. The controller convert to **deserialize** this JSON into native freamwork (language) data types. 
```json
{
  "name": "John",
  "email": "john@example.com",
  "age": 30
}
```
2. **~={purple}Validate=~**: -> The handler checks if the incoming data makes sense:
	- Is the email field actually an email?
	- Are required fields present?
	- Is the age a valid number?

3. **~={purple}Transform=~**: -> The handler prepares data for the backend:
	-  Normalize email to lowercase User@Gmail. com -> user@gmail. com
	- Add country code to phone (“9876543210” → “+919876543210”)
	- Standardize date format (“5/11/2025” → “2025-11-05”)

4.  ~={purple}**Delegation to Service Layer** =~
	- The handler passes the clean, validated, transformed data to the **Service layer**—the actual business logic processor.

5.  ~={purple}**Sending the Response** =~
	- Once the Service layer returns results
	- Decides the HTTP status code:
		- `200 OK`: Successful request
		- `201 Created`: Resource successfully created
		- `204 No Content`: Successful deletion (empty body)
		- `400 Bad Request`: Client error
		- `401 Unauthorized`: Authentication failed
		- `403 Forbidden`: Permission denied
		- `500 Server Error`: Internal error
----

## ~={red}Part 3=~
---
### ~={orange}The Brain - The Service Layer=~
- The **Service** layer is where the actual business logic of your application lives.

- The collection of plain function that perform specific tasks. a well-designed service function is completely agnostic of HTTP. it doesn't know about request , responses and status codes. it handle core application login 
- Operation like calculations, external API calls, email sending, notifications, or database interactions.  
	- ~={purple}Orchestration=~ :  It can call multiple repository methods, merge the results, and  perform complex operations. For example, a `PlaceOrder` service might call a repository to check stock, another to charge a payment gateway, and another to create the order record
	-  ~={purple}**Executing Core Logic=~:** It performs calculations, enforces business rules, and makes decisions.
	-  ~={purple}**Calling External Services:**=~ It can interact with external APIs, send emails, or publish messages to a queue.

---
## ~={red}Part 4=~
---
###  ~={orange}The Database Specialist - The Repository Layer.=~
- The Repository layer has **one job**: talk to the database.

What it does 
- Take some data, perform a database operation, and return result
- 1. The controller receives it from the request
- 2. The service layer passes in down.
- 3. The repository layer uses it to build the actual database query.
- 4. The database returns the result and the repository return it upward without processing it.

The One function, only one type of data
- Two needs 
	- Where passing ID return a single book
	- It returns all books

|Layer|Responsibility|
|---|---|
|**Handler / Controller**|Deals with data formats (input and output)|
|**Service**|Does all the actual processing|
|**Repository**|Makes all the database calls|


![[Screenshot From 2026-10-27 11-35-43 1.png]]
