1. Hoisting  
2. Differences between ~={red} `let`=~, ~={2}`var`=~ or ~={2}`const `=~   
3. difference between `==` and `===`    
4. Event Loop in JavaScript?  
5. Function Declarations vs. Expressions 
6. What Is Event Delegation?  
7. How Does ~={cyan}`this`=~ Work in JavaScript? 
8. ~={red}`localStorage`=~ vs ~={red}`sessionStorage ` =~  
9. How Do~={yellow} `null`=~, ~={yellow}`undefined`=~, and ~={yellow}`Undeclared Variables`=~ and ~={yellow}`NaN`=~ Differ?  
10. ~={pink}`.call()`=~ vs~={pink} `.apply()`=~ vs ~={pink}`.bind()`=~.  
11. Prototypal Inheritance  
12. What Are Higher-Order Functions?  
13. What are Events in JavaScript?  
14. What Is Event Capturing? 
15. ~={yellow}`Synchronous` =~vs ~={yellow}`Asynchronous`=~ Functions and ~={yellow}`Differ`=~? 
16. `Data Types` in JavaScript? 
17. `Call-back function`? 
18. `Debouncing` vs `Throttling`? 
19. What is `Lexical Scoping` ?   
20.  What is ~={red}`Scope` =~in JavaScript?
21. `Constructor` in JavaScript?
22. ~={red} `Factory`=~ Functions?
23.  What are~={2} `Promises`=~ and ~={2}`Async /await`=~?
24. What are ~={yellow}`arrow`=~ functions?
25. What is~={red} `IIFE `=~(Immediately Invoked Function Expression)?
26. What is ~={green}`callback`=~ in JavaScript?
27. What is Function ~={red}` currying`=~ in JavaScript and show `infinite currying`?
28. What are ~={pink}`closures`=~ in JavaScript?
29. Different between~={red} `setTimeout()` =~and~={red} `setInterval()`=~
30. What Are the Different` Types of Errors` in JavaScript?
31. Is JavaScript a~={1} `Statically-Typed` =~or ~={1}`Dynamically-Typed Language`=~?
32. What Is ~={1}`Memorization` =~in JavaScript?
33. What Are ~={blue}`Promises` =~in JavaScript?
34. What is  ~={cyan}`Promise.all()`=~ does?
35. what is ~={yellow}`deep copy` =~and~={yellow} `slow copy`=~?
36. `JSON.stringify` vs `JSON.parase`? 
37. tell me about ~={cyan}`Array method`=~  
38. tell me about ~={cyan}`String method`=~
39.  What is ~={purple}`temporal dead zone`=~ ?
40. What is~={red} `Pure function`=~ and ~={red}`Impure function`=~. 
41. Different between ~={4}`export`=~ and ~={4}`default export`=~.
42. Different between ~={red}`Splice Method`=~ and ~={red}`Slice Method`=~.    
43. truthy value and falsie value    
44. ~={1}`REST`=~ operator vs~={1} `SPREAD` =~operator in JavaScript.   
45. What is use ~={blue}`new`=~ keyword ?





---
## ~={1}1. What is Hoisting?=~

**Hoisting** is JavaScript's behaviour where declarations are processed before the code is executed.
####  Variable hoisting
`var` is hoisted and initialized with `undefined`.

```ts
console.log(a); // undefined

var a = 10;
console.log(a); // 10
```

You can think of it roughly as:

```ts
var a;

console.log(a); // undefined
a = 10;
```

#### `let` and `const`

`let` and `const` are also hoisted, but they are **not initialized** until execution reaches their declaration.

The time between entering the scope and reaching the declaration is called the ~={2}`Temporal Dead Zone (TDZ)`=~.

```ts
// TDZ starts
let name;

// TDZ ends when initialization happens
name = "John";
```

```ts
console.log(a); // ReferenceError
let a = 10;

console.log(b); // ReferenceError
const b = 20;
```

### Function hoisting

Function declarations are fully hoisted.

```ts
sayHello();

function sayHello() {
  console.log("Hello");
}
```

This works because the function declaration is available before its position in the source code is reached.


---

# 2. Difference between `let`, `var`, and `const`

|Feature|`var`|`let`|`const`|
|---|---|---|---|
|Scope|Function|Block|Block|
|Can be reassigned?|Yes|Yes|No|
|Can be redeclared in same scope?|Yes|No|No|
|Hoisted?|Yes|Yes, TDZ|Yes, TDZ|
|Must initialize immediately?|No|No|Yes|

### Example

```ts
var a = 10;
var a = 20; // allowed
```

```ts
let b = 10;
b = 20; // allowed

// let b = 30; // Error
```

```ts
const c = 10;

// c = 20; // Error
```

### Important point about `const`

`const` prevents reassignment of the variable, but objects themselves can still be modified.

```ts
const user = {
  name: "John"
};

user.name = "Mike"; // allowed

// user = {}; // Error
```

### Scope difference

```ts
if (true) {
  var x = 10;
  let y = 20;
}

console.log(x); // 10
console.log(y); // ReferenceError
```

**Interview answer:** Prefer `const` by default, use `let` when reassignment is needed, and generally avoid `var` in modern JavaScript.

---

# 3. Difference between `==` and `===`

### `==` — Loose Equality

`==` compares values after performing **type coercion** when necessary.

```ts
5 == "5"; // true
```

JavaScript converts `"5"` to a number.

### `===` — Strict Equality

`===` compares both **value and type**.

```ts
5 === "5"; // false
```

Because:

```ts
5       // number
"5"     // string
```

### More examples

```ts
0 == false;    // true
0 === false;   // false

null == undefined;   // true
null === undefined;  // false
```

In most production code, prefer:

```ts
===
```

because it avoids surprising type coercion.

---

# 4. Event Loop in JavaScript
Call Stack 
- Call stack is a actual JavaScript Engine 
- We need to code before push into the Call Stack
- Call Stack is waits for nothing.  

Web-API 
-   ~={2}`SetTimeout()`=~ is not part of JavaScript engine it is part of web-browser 
- 

Task Queue
- 

Micro-Task Queue
- 

Event loop 
- 

---

# 5. Function Declarations vs Function Expressions

## Function Declaration

A function declaration uses the `function` keyword directly.

```js
function add(a, b) {
  return a + b;
}

console.log(add(2, 3)); // 5
```

It is hoisted, so this also works:

```js
console.log(add(2, 3));

function add(a, b) {
  return a + b;
}
```

## Function Expression

A function is assigned to a variable.

```js
const add = function (a, b) {
  return a + b;
};

console.log(add(2, 3));
```

With `const`, you cannot use the function before initialization:

```js
add(2, 3); // ReferenceError

const add = function (a, b) {
  return a + b;
};
```

### Main difference

**Function declaration:**

```js
function greet() {}
```

**Function expression:**

```js
const greet = function () {};
```

Arrow functions are also function expressions:

```js
const greet = () => {
  console.log("Hello");
};
```

---

# 6. Event Delegation and Event Capturing

## Event Delegation

**Event delegation** means attaching one event listener to a parent instead of adding separate listeners to every child.

Example:

```HTML
<ul id="users">
  <li>John</li>
  <li>Alex</li>
  <li>Sam</li>
</ul>
```

Instead of:

```ts
document.querySelectorAll("li").forEach(item => {
  item.addEventListener("click", () => {
    console.log("Clicked");
  });
});
```

Use delegation:

```ts
document.querySelector("#users").addEventListener("click", (event) => {
  if (event.target.tagName === "LI") {
    console.log("Clicked:", event.target.textContent);
  }
});
```

This is useful because one listener can handle many children, including elements added later.

---

## Event Capturing

When an event occurs, it travels through phases.

Simplified:

```
Window
  ↓
Document
  ↓
Parent
  ↓
Child
  ↑
Parent
  ↑
Document
```

The first part is **capturing phase**.

The event then reaches the target.

After that, it travels back during the **bubbling phase**.

### Capturing example

```ts
parent.addEventListener(
  "click",
  () => {
    console.log("Parent");
  },
  true
);
```

The third argument `true` means use the capturing phase.

### Bubbling

By default:

```
parent.addEventListener("click", () => {
  console.log("Parent");
});
```

uses bubbling.

### Simple distinction

**Capturing:** parent → child

**Bubbling:** child → parent

**Delegation:** commonly uses bubbling to let a parent handle events from its children.

---

# 7. How does `this` work in JavaScript?

`this` refers to the object/context associated with the current function invocation. Its value depends mainly on **how the function is called**, not where the function was written.

### Object method

```js
const user = {
  name: "John",
  greet() {
    console.log(this.name);
  }
};

user.greet(); // John
```

Here `this` refers to `user`.

### Regular function

In strict mode:

```js
function test() {
  console.log(this);
}

test(); // undefined
```

In non-strict browser code, a plain function call can result in `this` referring to the global object.

### Arrow functions

Arrow functions **do not create their own `this`**. They inherit `this` from the surrounding lexical scope.

```ts
const user = {
  name: "John",

  greet() {
    const test = () => {
      console.log(this.name);
    };

    test();
  }
};

user.greet(); // John
```

### `this` with `new`

```ts
function Person(name) {
  this.name = name;
}

const person = new Person("John");

console.log(person.name); // John
```

With `new`, `this` refers to the newly created object.

### Interview tip

A good rule is:

> For a normal function, look at how it is called to determine `this`. For an arrow function, look at the surrounding lexical scope.

---

# 8. `localStorage` vs `sessionStorage`

Both are browser Web Storage APIs.

|Feature|`localStorage`|`sessionStorage`|
|---|---|---|
|Lifetime|Persists until removed|Usually lasts for the page session|
|Survives page refresh?|Yes|Yes|
|Survives closing browser/tab?|Generally yes|Usually no|
|Shared across tabs of same origin?|Yes|Separate per tab/session|
|Capacity|Browser-dependent|Browser-dependent|

### localStorage

```js
localStorage.setItem("name", "John");

console.log(localStorage.getItem("name"));

localStorage.removeItem("name");
```

### sessionStorage

```js
sessionStorage.setItem("name", "John");

console.log(sessionStorage.getItem("name"));

sessionStorage.removeItem("name");
```

Both store **strings**.

For objects:

```js
const user = {
  name: "John",
  age: 20
};

localStorage.setItem("user", JSON.stringify(user));

const savedUser = JSON.parse(
  localStorage.getItem("user")
);

console.log(savedUser.name);
```

Don't use Web Storage for highly sensitive information merely because it is convenient; it is accessible to JavaScript running on the page.

---

# 9. `null`, `undefined`, Undeclared Variables, and `NaN`

These four are different concepts.

## `undefined`

A variable exists but currently has no assigned value.

```js
let x;

console.log(x); // undefined
```

A function that doesn't explicitly return anything also returns `undefined`.

```js
function test() {}

console.log(test()); // undefined
```

---

## `null`

`null` is an intentional representation of **no value**.

```js
let user = null;
```

It usually means:

> "There is deliberately no value here."

Interesting JavaScript behavior:

```js
typeof null; // "object"
```

This is a historical language quirk.

---

## Undeclared variable

An undeclared variable has **not been declared at all**.

```js
console.log(x);
```

If `x` was never declared, this causes:

```js
ReferenceError
```

Compare:

```js
let x;

console.log(x); // undefined
```

versus:

```js
console.log(y); // ReferenceError
```

---

## `NaN`

`NaN` means **Not-a-Number** and represents an invalid numeric result.

```js
const result = "hello" / 2;

console.log(result); // NaN
```

Interestingly:

```js
typeof NaN; // "number"
```

To test for it, prefer:

```js
Number.isNaN(value);
```

Example:

```js
Number.isNaN(NaN); // true
Number.isNaN("hello"); // false
```

### Summary

```js
undefined → declared but no value assigned

null      → intentionally empty value

undeclared → variable doesn't exist/declaration was never made

NaN       → invalid numeric result
```

---

# 10. `.call()` vs `.apply()` vs `.bind()`

All three can control the `this` value of a function.

Suppose:

```js
const user = {
  name: "John"
};

function greet(age, city) {
  console.log(this.name, age, city);
}
```

## `.call()`

Calls the function **immediately**.

Arguments are passed individually.

```
greet.call(user, 25, "Delhi");
```

Output:

```
John 25 Delhi
```

---

## `.apply()`

Also calls the function **immediately**.

But arguments are provided as an array (or array-like value).

```
greet.apply(user, [25, "Delhi"]);
```

Output:

```
John 25 Delhi
```

---

## `.bind()`

Does **not** call the function immediately.

Instead, it returns a new function with `this` bound.

```
const boundGreet = greet.bind(user, 25, "Delhi");

boundGreet();
```

Output:

```
John 25 Delhi
```

### Main difference

```
call  → invokes immediately, arguments individually
apply → invokes immediately, arguments as array/array-like
bind  → returns a new function for later invocation
```

### Practical example

```js
const person = {
  name: "John"
};

function sayHello(age) {
  console.log(`Hello ${this.name}, age ${age}`);
}

sayHello.call(person, 25);

sayHello.apply(person, [25]);

const fn = sayHello.bind(person, 25);
fn();
```

All three let you control `this`, but their invocation behavior differs.

---
## 11. What is Prototypal Inheritance?

**Prototypal inheritance** is a mechanism where an object can inherit properties and methods from another object through its **prototype**.

JavaScript objects can have an internal link to another object called their prototype.

### Example

```js
const person = {
  greet() {
    console.log("Hello!");
  }
};

const student = Object.create(person);

student.name = "John";

student.greet(); // Hello!
```

Here:

```js
student
   ↓
person
```

When JavaScript doesn't find `greet()` directly on `student`, it looks at `student`'s prototype (`person`).

### Constructor example

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function () {
  console.log(`Hello ${this.name}`);
};

const user1 = new Person("John");
const user2 = new Person("Alex");

user1.sayHello();
user2.sayHello();
```

Both objects share the same `sayHello` method through the prototype rather than each having a separate copy.

### Interview answer

> Prototypal inheritance is JavaScript's object inheritance mechanism where objects inherit properties and methods from another object through the prototype chain.

---

# 12. What are Higher-Order Functions?

A **Higher-Order Function (HOF)** is a function that does at least one of these:

1. Takes another function as an argument.
2. Returns another function.

### Function as an argument

```js
function greet(name) {
  return `Hello ${name}`;
}

function processUser(name, callback) {
  return callback(name);
}

console.log(processUser("John", greet));
```

Here `processUser()` is a higher-order function because it receives `greet` as an argument.

### Function returning another function

```js
function multiplier(x) {
  return function (y) {
    return x * y;
  };
}

const double = multiplier(2);

console.log(double(5)); // 10
```

`multiplier()` returns a function, so it is a higher-order function.

### Common examples

JavaScript's array methods are frequently higher-order functions:

```js
const numbers = [1, 2, 3, 4];

const doubled = numbers.map(function (num) {
  return num * 2;
});

console.log(doubled);
```

`map()` receives a function, so `map()` is a higher-order function.

Common HOFs include:

```
map()
filter()
reduce()
forEach()
some()
every()
```

---

# 13. What are Events in JavaScript?

An **event** is an action or occurrence that JavaScript can detect and respond to.

Examples include:

```
click
keydown
keyup
submit
input
mouseover
load
resize
scroll
```

### Example

HTML:

```
<button id="btn">Click Me</button>
```

JavaScript:

```
const button = document.getElementById("btn");

button.addEventListener("click", function () {
  console.log("Button clicked");
});
```

When the user clicks the button, a `click` event occurs and the callback runs.

### Event object

JavaScript provides information about the event through an event object.

```
button.addEventListener("click", function (event) {
  console.log(event.type);
  console.log(event.target);
});
```

`event.target` tells you which element triggered the event.

### Interview answer

> An event is an action or occurrence in the browser, such as a click, key press, or form submission, that JavaScript can detect and handle using event listeners.

---

# 14. What is Event Capturing?

**Event capturing** is the phase where an event travels from the outermost element toward the element that triggered the event.

Suppose we have:

```
<div id="parent">
  <button id="child">Click</button>
</div>
```

When the button is clicked, the event travels roughly like:

```
Window
  ↓
Document
  ↓
html
  ↓
body
  ↓
div
  ↓
button
```

This is the **capturing phase**.

### Example

```
const parent = document.getElementById("parent");

parent.addEventListener(
  "click",
  function () {
    console.log("Parent capturing");
  },
  true
);
```

The `true` indicates that the listener should run during capturing.

Compare this with the default:

```
parent.addEventListener("click", function () {
  console.log("Parent bubbling");
});
```

The default is the **bubbling phase**.

### Easy way to remember

```
Capturing → Parent → Child

Bubbling  → Child → Parent
```

---

# 15. Synchronous vs Asynchronous Functions

## Synchronous

Synchronous code executes **one operation at a time** and waits for the current operation to finish before moving to the next.

```
console.log("Start");

function add(a, b) {
  return a + b;
}

console.log(add(2, 3));

console.log("End");
```

Output:

```
Start
5
End
```

Everything happens sequentially.

---

## Asynchronous

Asynchronous operations allow JavaScript to start an operation and continue executing other code while waiting for the asynchronous operation to complete.

Example:

```
console.log("Start");

setTimeout(() => {
  console.log("Async operation");
}, 2000);

console.log("End");
```

Output:

```
Start
End
Async operation
```

The timer callback doesn't block the rest of the code.

### Common asynchronous mechanisms

```
setTimeout()
setInterval()
Promises
async / await
fetch()
DOM events
```

### Important distinction

`async` doesn't mean "runs on another JavaScript thread."

It means the operation can be handled asynchronously, allowing the main JavaScript execution to continue.

### Interview answer

> Synchronous code executes sequentially and waits for each operation to complete. Asynchronous code allows other work to continue while an operation is waiting, with the result handled later through callbacks, promises, or async/await.

---

# 16. Data Types in JavaScript

JavaScript has **primitive types** and **non-primitive/reference types**.

## Primitive Data Types

There are 7 primitive types:

```
1. String
2. Number
3. BigInt
4. Boolean
5. Undefined
6. Null
7. Symbol
```

### String

```
const name = "John";
```

### Number

JavaScript uses the `number` type for regular integer and floating-point numbers.

```
const age = 25;
const price = 99.99;
```

### BigInt

Used for integers larger than the safe range of the regular `Number` type.

```
const bigNumber = 12345678901234567890n;
```

### Boolean

```
const isLoggedIn = true;
```

### Undefined

```
let x;

console.log(x); // undefined
```

### Null

```
const user = null;
```

### Symbol

Used to create unique symbol values.

```
const id = Symbol("id");
```

---

## Reference/Non-Primitive Types

The main reference type is:

```
Object
```

Objects include things such as:

```
const user = {
  name: "John",
  age: 25
};
```

Arrays and functions are also objects in JavaScript's type system:

```
const numbers = [1, 2, 3];

function greet() {
  console.log("Hello");
}
```

### Interview trick

```
typeof null;       // "object"
typeof [];         // "object"
typeof function(){}; // "function"
```

These are JavaScript-specific behaviors worth remembering.

---

# 17. What is a Callback Function?

A **callback function** is a function passed into another function so that it can be called later.

### Simple example

```
function greet(name, callback) {
  console.log(`Hello ${name}`);
  callback();
}

function done() {
  console.log("Finished");
}

greet("John", done);
```

Output:

```
Hello John
Finished
```

Here:

```
done
```

is the callback.

### Asynchronous example

```
setTimeout(() => {
  console.log("Executed later");
}, 1000);
```

The arrow function is a callback passed to `setTimeout()`.

### Callback vs Higher-Order Function

These concepts are related but aren't the same.

```
function process(callback) {
  callback();
}
```

`process()` is a **higher-order function** because it accepts a function.

The function passed to it is a **callback function**.

---

# 18. Debouncing vs Throttling

Both are techniques for controlling how often a function executes when an event happens repeatedly.

## Debouncing

**Debouncing** waits until the repeated events stop, then executes the function.

A common example is a search box.

```
function debounce(fn, delay) {
  let timer;

  return function (...args) {
    clearTimeout(timer);

    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  };
}
```

Usage:

```
const search = debounce((value) => {
  console.log("Searching:", value);
}, 500);
```

If the user types:

```
J
Jo
Joh
John
```

quickly, the search function can run once after the user stops typing for 500ms.

### Debounce

```
Event Event Event Event
              ↓
          wait...
              ↓
           Execute
```

---

## Throttling

**Throttling** ensures a function executes at most once during a specified time interval.

This is useful for events such as scrolling or resizing.

```
function throttle(fn, delay) {
  let lastTime = 0;

  return function (...args) {
    const now = Date.now();

    if (now - lastTime >= delay) {
      lastTime = now;
      fn.apply(this, args);
    }
  };
}
```

### Throttle

```
Event Event Event Event Event Event
 ↓        ↓        ↓
Execute  Execute  Execute
```

### Difference

|Debouncing|Throttling|
|---|---|
|Waits for events to stop|Executes at controlled intervals|
|Good for search input|Good for scroll/resize|
|Usually runs after the burst|Runs during the burst at a limited rate|

**Easy memory trick:**

> Debounce = "Wait until things calm down."

> Throttle = "Slow down how often it runs."

---

# 19. What is Lexical Scoping?

**Lexical scoping** means a function's access to variables is determined by **where the function is written in the source code**.

```js
const name = "John";

function greet() {
  console.log(name);
}

greet();
```

`greet()` can access `name` because `name` exists in its outer lexical environment.

### Nested example

```
function outer() {
  const message = "Hello";

  function inner() {
    console.log(message);
  }

  inner();
}

outer();
```

`inner()` can access `message` because `inner()` was defined inside `outer()`.

### Important connection to closures

Lexical scoping is the foundation for **closures**.

```
function counter() {
  let count = 0;

  return function () {
    count++;
    console.log(count);
  };
}

const increment = counter();

increment(); // 1
increment(); // 2
increment(); // 3
```

The returned function remembers the lexical environment in which it was created.

---

# 20. What is Scope in JavaScript?

**Scope** determines where a variable can be accessed in your code.

JavaScript commonly has:

```
Global Scope
Function Scope
Block Scope
Module Scope
```

## Global Scope

A variable declared outside functions/blocks can be globally accessible within its applicable environment.

```
const name = "John";

function greet() {
  console.log(name);
}
```

---

## Function Scope

Variables declared with `var` inside a function are available throughout that function.

```
function test() {
  var x = 10;

  console.log(x);
}

test();

// console.log(x); // Error
```

---

## Block Scope

`let` and `const` are block scoped.

```
if (true) {
  let x = 10;
  const y = 20;

  console.log(x);
}

// x and y cannot be accessed here
```

A block is typically defined by:

```
{
  // block
}
```

such as an `if`, `for`, or `while` block.

---

## Scope Chain

JavaScript searches for variables through nested lexical environments.

```
const a = 10;

function outer() {
  const b = 20;

  function inner() {
    const c = 30;

    console.log(a); // 10
    console.log(b); // 20
    console.log(c); // 30
  }

  inner();
}

outer();
```

The lookup works roughly like:

```
inner scope
    ↓
outer scope
    ↓
global scope
```

This is called the **scope chain**.
---
## 20. Debouncing vs Throttling?

---
## 21. What is Lexical Scoping?

- Lexical scoping is JavaScript refers to the way functions access variable based on their location in the they have code. it allows inner functions to access variables from their outer function. 

```js
function outerFunction() {
    var outerVar = 'I am outside!';
    
    function innerFunction() {
        console.log(outerVar); // Accesses outerVar from the parent scope
    }
    
    innerFunction();
}

outerFunction(); // Outputs: I am outside!
```

```jsx
var outerVar = 'I am outside!';

function innerFunction() {
  console.log(outerVar);
}

innerFunction();
```
---
## 22. What is Scope in JavaScript?

-  where to accessed variable, function 
### Types of Scope

1. Global Scope : In global scope can be used anywhere in we are program  

```js
let x = 10; // global

function show() {
  console.log(x); // can access
}

show();
console.log(x); // also works

```

2. Function Scope : Variable declared inside a function can only be used inside that function.  

```js
function test() {
  let y = 20;
  console.log(y); // works
}

test();
console.log(y); // ❌ error — y is not accessible here

```

3. Block Scope : Inside `{ }` using `let`/ `const`
```js
if (true) {
  let z = 30;
  console.log(z); // works
}

console.log(z); // ❌ z is not available outside the block

```

---
### 23. What is  `this `in JavaScript ? 

- this is a special keyword that refers to the current context
- that object that is executing the current code. 

---
### 24. JavaScript  Object? 

- Object is a collection of key-value pain. and Each key-value pair is called a property.

- the key of property can be a string,
- the value of property can be like string, number, array, and function.

- use the dot notion (.) and array like notation ([]) to access the property of an object. 

`````js
Ex let person ={
	fistName: "sandeep",
	lastName: "Suthar"
	
}
`````
---
 
### 25. Constructor in JavaScript? 
- In JavaScript a constructor is a special function used to create and initialize object. it's typically used with the new keyword 

```js
faction Person(name, age){
	this.name = name;
	this.age = age;
}
const a = new Person("sandeep", 10);
const b = new Person("karan", 20)

console.log(a.name)
console.log(b.age)
```

---

## 26. Factory Functions
A factory function is a function that return a new object it is an alternative to use Ing constructor function. 


`````js
function person(firstName, lastName){
	return {
		firstName: "sandeep";
		lastName: "suthar"
				
		getFullName(){
			return firstName +" "+lastName; 
			
		}
		
	}
}
`````

```js
let person1 = createPerson('John', 'Doe');
let person2 = createPerson('Jane', 'Doe');

console.log(person1.getFullName());
console.log(person2.getFullName());

```

`````js
function fancyAlert(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}
`````

----------------------------------------------------------------------

### 27. What are Promises and Async /await?
#### ~={yellow}Promises=~
A Promise is an Object that representing the eventual completion or failure of an asynchronous operation.

```jsx
const myPromise = new Promise((resolve, reject) => {
  let success = true;

  if (success) {
    resolve("Task completed!");
  } else {
    reject("Task failed!");
  }
});
```
 
 ### ~={yellow}Async /await=~
 
---
### 28. What are arrow functions?
Arrow function are provide a shorter syntax and they commonly used for cleaner code, they have~={orange} own `this`=~

```jsx
const sum = (a, b) => a + b;
```

---

### 29. What is IIFE (Immediately Invoked Function Expression)?
an IIFE is a function that runs immediately after declaration. 

```jsx
(function(){
	console.log("My Name is sandeep")
})();
```

---
###  30. What is callback in JavaScript?
A callback is a function passed as an argument to another function. usually Running code after some task is finished. 

mainly used for: 
- Asynchronous operation

```jsx
function calculate(a, b, callback) {
  callback(a + b);
}

calculate(2, 3, result => {
  console.log(result);
});
```

---

### 31. What is currying in JavaScript?

currying is a technique where a function that normally takes multiple arguments is transformed into a sequence of function that each take one argument. 

- use currying when one arguments stays same and other arguments can be changes 

```jsx
function add(a){
  return function(b){
    return a + b;
  }
}
console.log(add(5)(3)); // 8

// Instead of `add(2, 3)` → `add(2)(3)`
```

```js
const multiply = a => b => a * b;

const double = multiply(2);
const triple = multiply(3);

double(5); // 10
triple(5); // 15
```


---

### 32.  What are closures in JavaScript?
---
### 33. Different between~={red} `setTimeout()` =~and~={red} `setInterval()` =~. 
---
### 34.  What Are the Different Types of Errors in JavaScript?
---
###  35.  Is JavaScript a Statically-Typed or Dynamically-Typed Language?
---
### 36. What Is ~={orange}`Memorization` =~in JavaScript?
---
### 37.  What Are Promises in JavaScript?
---


---
### 44. 1. Different between ~={orange}`Splice Method`=~ and ~={orange}`Slice Method` =~.
- `slice()` : does not modify the original array - it return a new array containing the selected elements. 
```js
const fruits = ['apple', 'banana', 'cherry', 'date', 'elderberry'];
const Citrus fruits.slice(1, 3); // ['banana', 'cherry']
console.log(fruits); // ['apple', 'banana', 'cherry', 'date', 'elderberry'] - original unchanged
```

- `splice()` : modifies the original array in place by removing, adding, or replacing elements. 
#### Remove
```js
const fruits = ['apple', 'banana', 'cherry', 'date', 'fig'];

// Remove 2 elements starting at index 1
const removed = fruits.splice(1, 2);

console.log(removed); // ['banana', 'cherry'] - the removed elements
console.log(fruits);  // ['apple', 'date', 'fig'] - original array modified
```
#### Replacing
```js
const animals = ['cat', 'dog', 'bird', 'fish'];

// Replace 'dog' and 'bird' with 'rabbit' and 'hamster'
animals.splice(1, 2, 'rabbit', 'hamster');

console.log(animals); // ['cat', 'rabbit', 'hamster', 'fish']
```

#### Adding
```js
const colors = ['red', 'green', 'blue'];

// Add 'yellow' and 'purple' at index 1
colors.splice(1, 0, 'yellow', 'purple');

console.log(colors); // ['red', 'yellow', 'purple', 'green', 'blue']
```

------------------------------------------------------------- 

### 45. ~={red} `Truthy` =~value and ~={red}`Falsie`=~ value. 
**JavaScript**, values are automatically converted to `true` or `false` in Boolean contexts (like `if` statements). These are called ~={blue}`truthy`=~ and ~={blue}`falsy`=~ values.

```js 
false                   // Falsie value
0
-0
0n        // BigInt zero
""        // empty string
null
undefined
NaN


if (0) {
  console.log("Won't run");
}
```

```js
true                 // Truthy
1
-1
"0"
"false"
" "       // space
[]
{}
function(){}

```
---

### 46.  ~={red}`REST` =~operator vs ~={red}`SPREAD`=~ operator in JavaScript ✔️
- Used to spread elements. 
In Array
```js
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4];

console.log(arr2); // [1, 2, 3, 4]
```
 
 In Object
  ```js
const user = { name: "Alex", age: 20 };
const updatedUser = { ...user, age: 21 };

console.log(updatedUser);
  ```

- Used to collect multiple values into one. 
In function parameters
```js
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}

sum(1, 2, 3, 4); // 10
```

In Array destructuring
```js
const [first, ...rest] = [10, 20, 30, 40];

console.log(first); // 10
console.log(rest);  // [20, 30, 40]
```

In object destructuring
```js
const { name, ...otherDetails } = {
  name: "Alex",
  age: 20,
  country: "USA"
};

console.log(otherDetails); // { age: 20, country: "USA" }
```

----
### 47. What is use ~={blue}`new`=~ keyword ? ✔️
- The ~={blue}**`new`**=~ keyword is used to create instance of Object from a constructor function. 

 ```js
 function Person(name, age) {
  this.name = name;
  this.age = age;
}

const p1 = new Person("Rahul", 25);
 ```
  ---
 
 
 
 
 
 ## Prototype in Js

when we try to acces property or methos in object 
so first find at the object itself,
it is not found, it find at the object in prototype, this is chainig continues wntil it reache null. 



---------------------------------------------------------------

Prototype Inheritance
Js prototype Inheritance is the mechnisn that is object can inherit properties and methods form another ovhect through the prototype chaining. 


--------------------------------------------------------------------




------------------------------------------------------------------------

