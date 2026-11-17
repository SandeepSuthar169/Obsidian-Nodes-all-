
####  ~={red}Authentication=~ :  
- The Process of verifying "who are you" 
- e.g. Logging in with username and password
#### ~={red}Authorization=~ : 
- The process of verifying "What can you do?"
- e.g. An admin can delete posts but a regular user cannot. 

---
#### ~={red}Sessions=~ vs ~={red}JWT (JSON WEB Tokens)=~ vs ~={red}Cookies=~

- HTTP is used for client-server communication HTTP emerge for as the backbone of all client and server 
- By design HTTP is Stateless which means HTTP doesn't know about or knows about previous communication or request.
- When Static website move to or transition to dynamic website and that of the HTTP become a bottleneck. Example E-commerce like website. 

#### **~={purple}Sessions=~**
- Type :  Statefull 
- Works : Server create session ID -> stores user data in Database / Redis -> sends ID in cookie 
- Pros : real time control 
- Cons: Scalability issues
Phases 
 1. When someone first visits,  the server may create session, often just a random ID stored in cookie.
 2. When user login, Server verifies them 
 3. The server creates a new session and ties it to the user. 
	- Best practice is to generate a new session ID
	- Server-side session: The server stores session in In-memory, Redis, DB and sends the browser only a random session ID in a cookie
4. Session storage on the client
	- The ID or token is stored in the browser, Ideally in a HTTP only JavaScript can't read it 
	- Store tokens in Local Storage.
5. Validation every request
	 - Each request carries the cookie or token automatically, The server checks that session ID is exists or expired. 
6. Renewal 
	 - Refresh Tokens: a short-lived access token (ex. 15 Minutes ).
7. Termination
	 - Logout: The server deletes the session, cookie, token
	 - Expiry: When time to reach end of timeout.

---
#### ~={purple}**JWT (JSON WEB Tokens**) =~
- Type :  Stateless
- ~={orange}Works=~ : Self-contained token ( header + payload + signature with secret key )
- ~={orange}Pros=~ : No server store and highly scalable 
- ~={orange}Cons=~: Hard to Cancel 

JWT is a signed string the server gives the browser after login
- ~={green}Header=~: the signing algorithm ( HS256 )
- ~={green}payload ( Claims )=~: Data like sub ( user ID ), role, iat (isssued at), exp (expiry)
- ~={green}Signature=~: A hash of header + payload made with a secret. 

~={cyan} **Access token=~:** short-lived (about 5 to 15 minutes), used for API calls
 ~={cyan}**Refresh token=~:** long-lived (days or weeks), used only to get new access tokens
 
 ----
#### **~={purple}Cookies=~** -> A browser mechanism for storing data sent by a server. 
- Type :  Transport 
- Works : Browser automatically sends cookie with every request to the same domain 
- Pros : Automatic 
- Cons: size limits 

A Cookie is small piece of data (name and value,)

#### How it Work
1. The browser sends a request for example POST/ auth/login
2. The Server response includes a header:
```
Set-Cookie: refresh_token=abc123; HttpOnly; Secure; SameSite=Lax; Path=/auth; Max-Age=604800
```

3. The browser saves the cookie
4. On every later request that matches the cookie's domain and path, the browser adds:
```
Cookie: refresh_token=abc123
```

---
### ~={red}Types of Authentication=~

| ~={orange}Type=~       | ~={orange}Best For=~                      | ~={orange}Mechanism=~               | ~={orange}Pros=~                       | ~={orange}Cons=~          |
| ---------------------- | ----------------------------------------- | ----------------------------------- | -------------------------------------- | ------------------------- |
| **Stateful (Session)** | Web apps, SaaS                            | Session ID + server                 | real-time info                         | Scaling cost              |
| **Stateless (JWT)**    | APIs, mobile, microservices, SPAs         | JWT in Authorization header         | Infinite horizontal scaling            | Revocation difficult      |
| **API Key**            | Machine-to-machine                        | Long-lived secret key in header     | Simple, programmatic access            | No user context           |
| **OAuth 2.0 + OIDC**   | Third-party login (“Sign in with Google”) | Authorization code + ID token (JWT) | Secure delegation, no password sharing | More complex to implement |

---
#### ~={red}``Authorization – RBAC (Role-Based Access Control)``=~
- Users are assigned Roles (Admin, User, Students) and roles are assigned Permissions (Read , Write, Update, Delete).



---
#### ~={red}Security Best Practices =~            ~={cyan} ( MOST IMP  )=~
#### ~={green}A.=~ Never leak information in error messages
**Bad** (gives attackers clues):
- “User not found”
- “Incorrect password”

**Good** (always generic):
- “Authentication failed” (for all three cases: user doesn’t exist, wrong password, account locked)
#### ~={green}B.=~ Protect against Timing Attacks       
- If username fails → response is fast.
- If password fails → response is slower (because of hashing).
- Attacker can measure time difference → knows if username exists.

**Solutions**:
- Use **constant-time** password comparison functions.
- Or add artificial delay (e.g., `setTimeout` / `time.Sleep` ) on early failures so every auth attempt takes the same time.
```js 
const crypto = require('crypto');
const bcrypt = require('bcrypt'); 

const loginUser = asyncHandler(async (req, res) => {
    const { email, password } = req.body;
    if (!email || !password) {
      throw new ApiError(400, "User not verified & all fields are required");
    }
    
    try {
        const user = await User.findOne({ email });

        const dummyHash = '$2b$10$dummyhashfordummypassword'; 
        // Precompute a bcrypt hash for 'dummy'
		const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));

        if (!user) {
            // Protection: Add artificial delay to match bcrypt.compare time                 (e.g., 200-500ms, test your setup)
            await delay(300); // Adjust based on your hashing speed

      // Dummy constant-time comparison (useless but ensures no timing leak)
            await bcrypt.compare(password, dummyHash); 
            // Or use crypto.timingSafeEqual if not using bcrypt
            throw new ApiError(401, "Invalid credentials"); // Generic error
        }

        if (!user.isEmailVerified) {
          throw new ApiError(400, "Verify your email before loginprocess.");
        }

        const passwordValid = await user.isPasswordCorrect(password);
        if (!passwordValid) {
            throw new ApiError(401, "Invalid credentials"); // Generic error
        }

        const accessToken = user.generateAccessToken();
        const refreshToken = user.generateRefreshToken();
        user.refreshToken = refreshToken;
        await user.save({ validateBeforeSave: false });

        const cookieOption = {
            httpOnly: true,
            secure: true,
            maxAge: 24 * 60 * 60 * 1000 // One day
        };

        res.cookie("accessToken", accessToken, cookieOption);
        res.cookie("refreshToken", refreshToken, cookieOption);

        return res.status(200).json(
            new ApiResponse(
                200,
                {
                    user: {
                        id: user._id,
                        username: user.username,
                        email: user.email
                    },
                    accessToken,
                    refreshToken
                },
                "User logged in successfully."
            )
        );
    } catch (error) {
        throw new ApiError(500, "Failed to log in user.", error);
    }
});
```
