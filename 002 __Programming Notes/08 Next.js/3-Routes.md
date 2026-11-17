
### ~={2}What is redirect in Next.js=~
- ~={3}`redirect()`=~ is a function used to immediately redirect the user to another route on the server. 
- It stops the current execution and sends the user to another page.
#### It is commonly used for:
- ~={3}Authentication=~ checks
- Protecting routes
- Redirecting after actions


| Feature         | ~={green}`redirect()`=~ | ~={green}`Router.push()`=~ |
| --------------- | ----------------------- | -------------------------- |
| When used       | Server                  | Client                     |
| Navigation type | Immediate redirect      | Client navigation          |
| Stops execution | Yes                     | NO                         |
| Common use      | Auth protection         | Button navigation          |
