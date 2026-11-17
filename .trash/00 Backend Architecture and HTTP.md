
####  ~={red}What is a backend?=~

The backend is the like "~={orange}`invisible engine`=~"  the part of application that runs on a server and handles data processing, storage and part of the  business logic and works on application programming interfaces (APIs), servers, and databases. 
#### ~={red}How do backends work?=~
- A user does something on the UI or ( frontend ).
	-> click a button, login
- The frontend sends a request to the backend
- The backend
	- Receives the request 
	- Runs login in backend side.
	- In Backend Reads or writes data in a database/ 
- After The backend sends a response back
	- success, error 


####  ~={red}Why can’t we write backend logic in frontends?=~
1.  Security reason
	-  if backend login ( Ex password checks ) were in the frontend,  anyone could see or modify it. 
2. Performance reason
	- Fronted can't handle heavy tasks like processing millions of requests.

#### ~={red}DNS Server=~
- A DNS server translates human-friendly names into machine-friendly addresses. 
```txt
google.com -> 142.250.190.14 -> Frontend contenct 
```

####  ~={red}URL (Uniform Resource Locator) ?=~
- URL is full home address of something on the the internet. 

```ruby
https://www.example.com:443/users/42?active=true#profile
```
- ~={cyan}`https://`=~ -> Protocol
	-  `http` → normal (not secure)
	- `https` → secure (encrypted)
	- `ftp` → file transfer
	- `ws` / `wss` → web sockets
- ~={cyan}`www.example.com`=~ -> Domain name (Host)
	- ~={orange}`www.example.com`=~ → DNS → 93.184.216.34
- ~={cyan}`:443`=~  -> Port 
- ~={cyan}`/users/42`=~ -> Path
- ~={cyan}`?active=true`=~ -> Query parameters 

### ~={red}HTTP Protocol=~. 
 - ( Hyper Text Transfer Protocol  )
 - It's the set of rules that lets a client (browser) and a server talk to each other over the internet. 
 
 #### 1. ~={blue}Heart of HTTP=~ ( Hyper Text Transfer Protocol )
 -  ~={orange}**`Stateless`**=~
	> Server ~={red}does not remember past requests=~ each request must include all the necessary data such as authentication tokens, session information, requests type or methods.  
	
	- ##### ~={orange}Why HTTP is stateless=~
		- Easy to scale 
		- Makes servers simple
	
	- State Management techniques
		 - Cookies, Sessions, Tokens
 
- ~={orange} **`Client-server model`**=~
	 -  The client-server model is architecture where clients request services and servers process and return responses.  

- ~={orange}**`HTTP Message`**=~
	-  Request messages -> Send request by the client.  
	- Response messages -> Received by the client. 


#### ~={red}HTTP Methods=~

| Method            | Purpose                  |
| ----------------- | ------------------------ |
| ~={red}GET=~      | Fetch data               |
| ~={orange}POST=~  | Create data              |
| ~={yellow}PUT=~   | Replace data             |
| ~={green}PATCH=~  | Update partial           |
| ~={cyan}DELETE=~  | Remove                   |
| ~={pink}OPTIONS=~ | Check allowed operations |
| HEAD              | Headers only             |
#### ~={red}Response Status Codes =~
- 1xx – Informational
- 2xx – Success
- 3xx – Redirection
- 4xx – Client Error
- 5xx – Server Error