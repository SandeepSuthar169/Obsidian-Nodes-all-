## ~={red} What is Caching?=~
- Caching means storing a copy of data in a location that is faster to access than the original source.
	- Reduces latency (faster responses)
	- Decreases load on databases and other resources
	- Saves computational power
	- Can reduce costs (e.g., fewer external API calls

---
### ~={red}Caching a content at ~={orange}Edge=~ location =~
- An edge location (Pop, Point of Presence ) is a CDN server placed closed to users, in cities all over the world, and sending the response from the origin every time, we do it once and store the result at the edge.

What happens:
1. The first user in New York requests a video thumbnail. The edge has nothing yet (cache miss), so it asks the origin.
2. The origin does the heavy computation once and sends the data once.
3. The edge stores the result and sends it to the user.
4. The next thousand users nearby get it straight from the edge (cache hit), with no computation and no long-distance transfer.  

| Problem             | How edge caching solve it                                        |
| ------------------- | ---------------------------------------------------------------- |
| Heavy computation   | The result is computed once, then reused many times              |
| Heavy data transfer | Data travels a short distance from the edge, not from the origin |
| Origin load         | Most requests never reach your backend                           |
| Latency             | The user talks to a nearby server                                |
| Bandwidth cost      | Origin egress drops because the edge serves repeated requests    |

---
### ~={red}Disk storage is cheaper per GB, and caching (in-memory) storage is more expensive per GB. The trade-off is speed.=~
- Disks (HDD/SSD) const far less per GB than RAM, Roughly, RAM can be 10-50 x  more exprensive per GB than SSD,
- Caches like Redis or Mamcached keep data in RAM, Which is costly and limited in size. 

---

- There is a situation where it's either about doing a lot of heavy Computation and sending Heavy amount of data when a log of heavy 
- We want to avoid both heavy Computation and sending Heavy amount of data

---
## ~={red}3 Level of Caches=~
#### ~={blue}1. Network base caching=~
The goal is to avoid sending every request all the way to your origin sever.

### ~={purple}CDN ( Content Delivery Network ) ->=~ 
- A CDN is a group of serves spread across the world (Cloudflare, Akasail or cloudFront )
- It stores copies of static content such as images, videos, CSS, JavaScript, And API responses. 
- A user in India gets the file from a nearby CDN server instead of your origin server in the US, so it loads faster and your server handles less traffic. 

#### ~={purple}DNS (Domain Name System)=~
- DNS converts a domain name (Google.com) into an IP address.

### ~={blue}2. Hardware base caching=~
This is caching built into the physical components of a computer.
- CPU caches (L1, L2, L3)
- RAM as a cache for disk
- Disk cache/buffe
- The speed order is: CPU registers → L1 → L2 → L3 → RAM → SSD → HDD.

#### 3. Software base caching (Interact with Hardware )
This is caching we buil or configure in we are application. It uses the hardware (mostly RAM)


---
## ~={red}Types of Caches=~
#### ~={blue} 1. In-Memory Cache (Application-Level Cache)=~
- Stored inside the application memory
e.g. Redis, Memcached
Pros: 
 - Extremely fast (RAM based)
 - Reduces DB hits
 Cons: 
 - Data lost on restart
 - Limited by RAM

#### ~={blue}2. Distributed Cache=~
- Shared cache across multiple servers.
e.g. Redis, 
Use Cases: 
- Microservices Architectures
Pros: 
 - Scalable
 - Shared state across services
 Cons: 
 - Network overhead
 - Slightly shower than local memory cache
#### ~={blue}3. CDN (Content Delivery Network) Cache=~
- Caches static assets geographically closer to users
e.g.
- Cloudflare
Use Case
- Images, CSS, JS, HTML
pros:
- Reduces latency globally
Cons:
- Less useful for dynamic content 

#### ~={blue}4. Application-Level Object Cache=~
- Caches specific objects in code. 
- e.g. User Id, User, Frequently accessed objects, Lookup tables.


---
## ~={red}Caching Strategies=~
### ~={blue}1. Lazy Caching (Cache-Aside)=~
- Application checks cache 
- If Cache hit  -> return data
- if Cache miss -> fetch from database
- Store result in cache
- Return data to user
```txt
Client
  ↓
Application
  ↓
Check Cache
   ├─ Hit → Return
   └─ Miss → Query DB → Store in Cache → Return
```

```js
const mongoose = require("mongoose");
const { createClient } = require("redis");

// --- MongoDB Setup ---
mongoose.connect("mongodb://localhost:27017/mydb");

const userSchema = new mongoose.Schema({
  name: String,
  email: String
});

const User = mongoose.model("User", userSchema);

// --- Redis Setup ---
const redisClient = createClient();
redisClient.connect();

// --- Cache-Aside Function ---
async function getUser(id) {
  const cacheKey = `user:${id}`;

  // 1️⃣ Check Redis
  const cachedUser = await redisClient.get(cacheKey);

  if (cachedUser) {
    console.log("Cache hit");
    return JSON.parse(cachedUser);
  }

  console.log("Cache miss");

  // 2️⃣ Fetch from MongoDB
  const user = await User.findById(id).lean();

  if (!user) return null;

  // 3️⃣ Store in Redis with TTL (10 minutes)
  await redisClient.set(
    cacheKey,
    JSON.stringify(user),
    { EX: 600 } // 600 seconds = 10 minutes
  );

  return user;
}
```

### ~={blue}2. Write-Through Caching=~
 > Every write goes to th e cache **and** the database at the same time.
-  Application reads from cache. 
- If cache miss -> load from DB -> store in cache -> return
```txt
App
  ↓
Cache (write)
  ↓
Database (write)
```

### ~={purple}Cache-Aside Vs Write-Through=~

| Feature            | Cache-Aside           | Write-Through          |
| ------------------ | --------------------- | ---------------------- |
| Who updates cache? | Application (on read) | Application (on write) |
| Cache consistency  | May be stale          | Always fresh           |
| Write latency      | Faster                | Slightly slower        |
| Complexity         | Low                   | Medium                 |
```js
async function createOrUpdateUser(id, userData) {
  const cacheKey = `user:${id}`;

  // 1️⃣ Write to Database
  const user = await User.findByIdAndUpdate(
    id,
    userData,
    { upsert: true, new: true }
  ).lean();

  // 2️⃣ Write to Cache
  await redisClient.set(
    cacheKey,
    JSON.stringify(user),
    { EX: 600 } // 10 min TTL
  );

  return user;
}
```
- When data is updated (POST, PUT, PATCH), update the database and cache simultaneously
- This ensure the cache is always fresh.

---
## ~={red} Eviction Policies=~

Since caches have limited size, you must decide what to remove when the cache is full. Common policies:

- ~={purple}**LRU (Least Recently Used)=~:** Removes the item that hasn't been accessed for the longest time.
- ~={purple}**LFU (Least Frequently Used)=~:** Removes the item accessed the fewest times.
- ~={purple}**TTL (Time To Live):**=~ Items automatically expire after a set time (e.g., 1 hour). You can combine TTL with other policies.
---
### ~={red}Examples=~

### Session Storage with Redis
```js
const session = require('express-session');
const RedisStore = require('connect-redis')(session);

app.use(session({
  store: new RedisStore({ client }),
  secret: 'your-secret',
  resave: false,
  saveUninitialized: false,
  cookie: { secure: false, maxAge: 86400000 } // 1 day
}));

app.get('/login', (req, res) => {
  req.session.userId = 123;
  res.send('Logged in');
});

app.get('/profile', (req, res) => {
  if (req.session.userId) {
    res.send(`User ID: ${req.session.userId}`);
  } else {
    res.status(401).send('Not logged in');
  }
});
```

### External API Caching
```js
const axios = require('axios');

app.get('/weather/:city', async (req, res) => {
  const city = req.params.city;
  const cacheKey = `weather:${city}`;

  // 1. Check cache
  const cached = await client.get(cacheKey);
  if (cached) {
    return res.json(JSON.parse(cached));
  }

  // 2. Fetch from external API
  const apiKey = 'YOUR_API_KEY';
  const url = `https://api.weather.com/v1/${city}?apiKey=${apiKey}`;
  const response = await axios.get(url);
  const weatherData = response.data;

  // 3. Cache for 30 minutes (TTL)
  await client.setEx(cacheKey, 1800, JSON.stringify(weatherData));

  res.json(weatherData);
});
```

### Rate Limiting with Redis
```js
const rateLimit = require('express-rate-limit');
const RedisStore = require('rate-limit-redis');

const limiter = rateLimit({
  store: new RedisStore({
    client,
    prefix: 'rl:' // Redis key prefix
  }),
  windowMs: 60 * 1000, // 1 minute
  max: 50, // limit each IP to 50 requests per windowMs
  message: 'Too many requests, please try again later.'
});

app.use('/api/', limiter); // Apply to all API routes

app.get('/api/data', (req, res) => {
  res.json({ data: 'Sensitive data' });
});
```

---

### Eviction Policy

Common eviction policies
1. LRU (Least Recently Used): Removes the item that has not been accessed for the longest time.
2. LFU (Least Frequently Used): Removes the item with the fewest accesses overall. It keeps data that is consistently popular, even if it was not touched in the last few minutes.
3. FIFO (First In, First Out): Removes the oldest item that was added, regardless of how often it is used.
4. TTL (Time To Live): Each item has an expiry time and is removed when it runs out.
5. Random: Removes a random item. It is cheap to run and works surprisingly well in some cases.
6. MRU (Most Recently Used): Removes the most recently used item.
7. Session
