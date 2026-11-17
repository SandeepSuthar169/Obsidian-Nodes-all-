

Node.js is a JavaScript runtime environment build on Chrome ~={orange}`V8 engine`=~, ~={orange}`C++`=~  and ~={green}`LibUV` =~ library they use ~={green}`Event Loop`=~ and ~={green}`Tread Pool`=~.
- it is allow to run JavaScript code outside of a web browser.  

---
### Importing Modern

### Types
##### 1. ~={red}`Commonjs`=~ 
- it is synchronous in nature 
- Blocking the execution for the next line of code.
- On execution wrap in the IIFE
	- CommonJs moduls like 
```jsx
const secret = "hidden";
function add(a, b) {
  return a + b;
}
module.exports = { add };
```

- wraps it in an IIFE like
```jsx
(function (exports, require, module, __filename, __dirname) {
  
  const secret = "hidden";
  function add(a, b) {
    return a + b;
  }
  module.exports = { add };
});
```

- ~={orange}`export`=~  ->  module exports, initially points to same object 
- ~={orange}`require`=~  -> a function that import modules 
- ~={orange} `module`=~   -> current module represent  
-  ~={orange}`__filename`=~  -> path of the current file
-  ~={orange}`__dirname`=~  ->  path of the current directory 

###### 2. ~={red}`ESM`=~ 
- ~={green}`import`=~ and~={green} `export` =~syntax use 
- Asynchronous in nature
- they support~={green} `await`=~
```jsx
export function add(a, b) {
  return a + b;
}
```

```jsx
// app.js
import { add } from './math.js';
console.log(add(5, 3)); // Output: 8
```

---
 1. How does Node.js work?  ✔️
 2. What is NPM ✔️
 3. 



---

### ~={red}1. How does Node.js work?  =~ ✔️

  - Node.js is a~={pink} JavaScript runtime environment =~
  - Used as a backend server side language. 
  - Node.js works on a single-threaded architecture using the V8 Engine. 
  
  1. ~={purple}V8 Engine=~ :-> Compile JavaScript code into fast machine code. 
 2. ~={purple} Event Loop=~ : -> Handles asynchronous tasks ( like I/O, timers, requests ) without blocking the main tread. 
  3. ~={purple}Libuv library=~ :-> it is handles background tasks like file system operations and networking. 
  4.  it is allow to run JavaScript code outside of a web browser.  
  ---
### ~={red}2. What is NPM?=~   ✔️

NPM stands for the Node Package Manger. It is the Package manager for the Node.js environment. It is used to install, share and manage dependencies like (libraries,  tools or packages ).

---
### ~={red}3. Why is NodeJS `single-threaded`? =~ ✔️

NodeJS is single-threaded to 
- reduce multithread complexity 
- non-blocking code by the event loop  
- avoiding thread management process 

---

### 4.  What is Express.js   ✔️
- Express.js is a web framework build on top of Node.js
- Express.js Provide features 
	- Routing
	- Request & response handling 
	- Middleware
	- Error Handling

---
### 5. How do you create a basic HTTP server in Node.js?

```js
import express from "express"
const app = express()
const port = 4000

app.get("/", (req, res) => {
    res.send("hello world")
})

app.listen(port, () => {
    console.log(`server listen port on ${port}`);
})
```