#### ~={orange}1. **Logic Error**=~
- The code run but produces incorrect results.
- Incorrect algorithm implementation or not handled edge cases.

Why logic error does
- Misunderstanding requirement
- Implement Algorithm incorrectly 
- Don't think about Edge cases

#### ~={orange}2. Database Error=~

| Type                  | Cause                                  | Impact                  |
| --------------------- | -------------------------------------- | ----------------------- |
| Connection Errors     | Network down, pool exhausted           | Complete system outage  |
| Constraint Violations | Duplicate emails, missing foreign keys | Data integrity issues   |
| Query Errors          | Malformed SQL, timeouts                | 500 errors              |
| Deadlocks             | Circular dependencies in transactions  | Performance degradation |

####  ~={orange}3 **External Service Errors**=~
- Failures in third-party services ( Payment processors, Email providers, Authentication services  ), Network issues, Connection timeout, DNS failure, Network partition, authentication errors, rate limiting ( HTTP 429 ).

####  ~={orange}4. **Rate Limiting Error**=~
- When malicious user who sending abnormal amount of request in a particular time in Backend mechanism implemented that we can prevent intentions or bad intentions 
- ~={blue}429 too many request =~
-  ~={pink}Exponential Backup Strategy=~ First user reach to 429 status code wait for few minutes 

####  ~={orange}5. Service Outage / Service going down ****=~
- External service going down 
	 Example AWS, Redis, etc.

#### ~={orange}4. **Input Validation Errors** =~
- Bad data send by User
	- missing required fields, wrong formats, out of range values, Required fields validation error 

#### ~={orange}5. Configuration Errors=~
- Missing or incorrect environment variables, misconfigured services

![[Screenshot From 2026-11-05 17-29-59.png]]

---

### ~={red}Finding errors before they spreed (Proactive Error Detection)=~ 
- The best error handing starts before error happens 

- Health checker 
	- /health    
	- /status
- Database base health checker
	- query performance health checker
	- Data integrity health checker
- External Service health checker
	- Which health check kind of functionality for external service 
		- Properly Connect
		- Properly functioning
	Example -> Payment service (Last payment should be not log time takes)
	-> Authentication service (Last Login should be not log time takes) ets.

----

# ~={red}Handling Errors Gracefully=~
#### 1. Immediate error response 

#### 2. Containment and Graceful degradation 

#### 3. Error Recovery Strategies 

#### 4. Error propagation cibtrol

#### 5. Global error handling - final safety net

---





```txt
src/
├── middleware/
│   ├── errorHandler.js      ← Global error handler
│   └── validate.js          ← Input validation
├── errors/
│   └── AppError.js          ← Custom error class
├── routes/
│   └── auth.js              ← Auth routes
└── app.js
```
##### ~={red} errors/AppError.js =~ — Custom Error Class
```js
class ApiError extends Error {
    constructor(
        statusCode,
        message = "",
        errors = [],
        stack = ""
    ) {
        super(message);
        this.statusCode = statusCode;
        this.success = false;
        this.errors = errors;
        
        if (stack) {
            this.stack = stack;
        } else {
            Error.captureStackTrace(this, this.constructor);
        }
    }
}
export { ApiError };
```

##### ~={red} middleware/errorHandler.js=~ — Global Error Handler
```js
const AppError = require('../errors/AppError');

// Detect DB constraint violations (using Prisma/Postgres as example)
function handleDBError(err) {
  // Unique constraint violation (e.g., duplicate email)
  if (err.code === 'P2002') {
    return new AppError('A record with this value already exists.', 409);
  }
  // Foreign key constraint (referencing non-existent record)
  if (err.code === 'P2003') {
    return new AppError('Related record not found.', 400);
  }
  return null;
}

function errorHandler(err, req, res, next) {
  // Try to convert known DB errors to user-friendly AppErrors
  const dbError = handleDBError(err);
  if (dbError) return res.status(dbError.statusCode).json({ message: dbError.message });

  // Known operational errors (thrown with AppError)
  if (err instanceof AppError && err.isOperational) {
    return res.status(err.statusCode).json({ message: err.message });
  }

  // ⚠️ Unknown errors — DO NOT leak internal details!
  console.error('UNHANDLED ERROR:', {
    correlationId: req.headers['x-correlation-id'],
    path: req.path,
    error: err.message,  // log internally, never send to client
  });

  return res.status(500).json({ message: 'Something went wrong.' });
}

module.exports = errorHandler;
```
##### ~={red}routes/auth.js=~ — Secure Login (No Info Leakage)
```js
const express = require('express');
const bcrypt = require('bcrypt');
const AppError = require('../errors/AppError');
const router = express.Router();

// Simulated DB call
async function findUserByEmail(email) { /* ... */ }

router.post('/login', async (req, res, next) => {
  try {
    const { email, password } = req.body;

    if (!email || !password) {
      throw new AppError('Email and password are required.', 400);
    }

    const user = await findUserByEmail(email);

    // ✅ SECURITY: Same generic message whether user doesn't exist OR password is wrong
    // This prevents account enumeration attacks
    const GENERIC_AUTH_ERROR = 'Invalid email or password.';

    if (!user) {
      throw new AppError(GENERIC_AUTH_ERROR, 401);
    }

    const isPasswordValid = await bcrypt.compare(password, user.passwordHash);
    if (!isPasswordValid) {
      throw new AppError(GENERIC_AUTH_ERROR, 401);
    }

    // ✅ SECURITY: Log user ID, NOT email or password
    console.info('Login success', { userId: user.id, correlationId: req.headers['x-correlation-id'] });

    res.json({ token: '...' });
  } catch (err) {
    next(err); // Pass to global error handler
  }
});

module.exports = router;
```

##### ~={red} middleware/validate.js =~— Catch Bad Input Early
```js
const AppError = require('../errors/AppError');

// Example: validate user registration body
function validateRegister(req, res, next) {
  const { email, password, name } = req.body;

  if (!email || !email.includes('@')) {
    return next(new AppError('A valid email is required.', 400));
  }
  if (!password || password.length < 8) {
    return next(new AppError('Password must be at least 8 characters.', 400));
  }
  if (!name || name.trim() === '') {
    return next(new AppError('Name is required.', 400));
  }

  next(); // All good, proceed
}

module.exports = { validateRegister };
```

---
### ~={blue}Prevention: Proactive Error Detection=~

- ~={purple}**Health Checks**=~: Expose an endpoint (e.g., `/health`) that returns a 200 status if the service is running. But go beyond that:
    - Test database connectivity (run a simple query).
    - Verify external services (e.g., send a test email, generate a test token).
    - Check that critical configuration and caches are loaded.
        
- ~={purple}**Start-up Validation**: =~Before the server starts, ensure all required environment variables and dependencies are available. This prevents runtime failures that would otherwise surprise users.

---
###  ~={blue}Monitoring and Observability=~
- ~={purple}**Logging**=~: Use structured logs (JSON) to include metadata. Avoid logging sensitive data (passwords, credit card numbers). Logs should provide enough context for debugging.
    
- ~={purple}**Metrics**=~: Track not only error rates but also performance metrics (response times, resource usage) and business metrics (successful transactions). A sudden drop in successful transactions might signal a problem even if error rates are normal.
    
- ~={purple}**Tools**:=~ Mentioned tools like Grafana, Loki for log aggregation and visualization.
---
### ~={blue}Key Philosophies / Strategies=~

1. ~={purple}**Immediate Response**=~
    - Recoverable (network, rate-limit, temp DB connection shortage) → retry + exponential backoff.
    - Non-recoverable → contain damage, graceful degradation, fallback.
2. ~={purple}**Error Recovery**=~
    - Auto: restart service, clean cache, switch to backup.
    - Manual: document runbooks and test them.
3. ~={purple}**Propagation Control**=~
    - Bubble errors up with context (never let raw DB errors reach the client).
    - Use separate processes / message queues / timeouts as error boundaries.
4. ~={purple}**Global Error Handling (the star of the talk)**=~
    - One middleware that catches **everything**.
    - Classify error → decide HTTP status + user-friendly message.
    - Advantages: no duplication, no forgotten cases, consistent security.
5. ~={purple}**Security Rules**=~
    - Never leak table names, stack traces, internal details, or sensitive data (email, passwords, cards) in responses or logs.
    - For login: always say “Invalid email or password” (never “User not found”).
    - Log user ID + correlation ID only.
---

Error messages 
- Something went wrong [ Client side ]
- Internal server error [ Server side ]

---

Logs 

---
End