1. What are components in React   ✔️
2. Differences  between functional and class components  ✔️
3. What is JSX ? Why is it used? ✔️

---
### ~={red}1. What are components in React  =~ ✔️
is a piece of the user interface (UI) in the information can be changed and that can be repeated called as component.  
- Each component what shows on the screen. 
	- A Header
	- A button 
	- A Profile Card

-  Types of React components
		1. Function Components 
		2. Class Components

---
### ~={red}2. Differences  between functional and class components.=~ ✔️

| Aspect      | Functional Components | Class Components  |
| ----------- | --------------------- | ----------------- |
| Syntax      | Function              | ES6 Class         |
| `this`      | ❌ No                  | ✅ Yes             |
| Boilerplate | Low                   | High              |
| Preferred   | ✅ Yes                 | ❌ Legacy          |

---
### ~={red}3. What is JSX ? Why is it used?=~ ✔️
A React component is a function that return some JSX.

JSX is an extension to JavaScript that allow we to write HTML-like directly inside JavaScript code. 

- Easier to read and maintain.
```jsx
function FullName(){
	return (
		<>
			<h1>
				Hello I am sandeep
			</h1>
		</>
	)
}
```

---
### ~={red}4. Difference between Real DOM and Virtual DOM.=~ ✔️
~={orange}1. Real DOM=~
-  The actual DOM (~={red}`Document Object Model `=~) maintained by the ~={red}browser.=~
 - Represents the UI as a tree of nodes
- Any change updates the browser UI directly

~={orange}2. Virtual DOM =~
-  A **lightweight JavaScript copy** of the Real DOM
-  Used by libraries like **React**

- ==how React compares the old UI with the new UI and updates only what changed.==

---
### ~={red}5. What is the significance of the`key` prop? =~ . ✔️
- Keys help React identify which is unique elements in a list  changed when update and render elements .
- Without keys React many re-render unnecessarily. 

```jsx
const items = [
  { id: 1, value: 'Item 1' },
  { id: 2, value: 'Item 2' },
  { id: 3, value: 'Item 3' },
];

function ItemList() {
  return (
    <ul>
      {items.map((item) => (
        <ListItem key={item.id} value={item.value} />
      ))}
    </ul>
  );
}

function ListItem({ value }) {
  return <li>{value}</li>;
}
```
---
### ~={red}6. Explain Props in React=~.  ✔️
- Props (means Properties) are how one components send data from another or child component.
- Props are read-only (the child cannot be change them)

```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

```jsx
function Component({ age, isStudent, hobbies, onClick }) {
  return (
    <div>
      <p>Age: {age}</p>
      {isStudent && <p>Student</p>}
      <button onClick={onClick}>Click</button>
      <ul>
	      {hobbies.map(hobby => (
		    <li key={hobby}>{hobby}</li>
	      ))}
	    </ul>
    </div>
  );
}
```
----
### ~={red}7. Difference between `Props` and `State` =~
1. ~={yellow}Props -> =~
- Props (means Properties) are how one components send data from another or child component.
- Props are read-only (the child cannot be change them)
```jsx
function App() {
  return <Student name="Alex" age={16} />;
}
        // paret to child

function Student({ name, age }) {
  return (
    <p>
      Name: {name}, Age: {age}
    </p>
  );
}
```
2.~={yellow} State ->=~
	- State is a stores dynamic data in a component. 
	- State is managed data within the component. 
	- When Changes to state trigger re-rendering of the component. 
	-  Can be changed
```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increase
      </button>
    </div>
  );
}
```

---
### ~={red}8. Explain the concept of lifting state up.=~
- Moving state from a child components to a parent component so multiple components can share and use the same data. 
```jsx  
// Parent Component
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <ChildA count={count} setCount={setCount} />
      <ChildB count={count} setCount={setCount} />
    </>
  );
}
```

```jsx 
// Child Components
function ChildA({ count, setCount }) {
  return <button onClick={() => setCount(count + 1)}>A: {count}</button>;
}

function ChildB({ count, setCount }) {
  return <button onClick={() => setCount(count + 1)}>B: {count}</button>;
}
```

---
### ~={red}9. What are fragments in React?=~
- Fragments are lets we are group multiple elements without adding extra DOM nodes.

```jsx
return (
  <>
    <h1>Hello</h1>
    <p>Welcome</p>
  </>
);
```

---
### ~={red}10. What are Synthetic events in React?=~
- synthetic Events are Reacts wrapper around native browser events to make event handling consistent across all browsers. 
```jsx
function Button() {
  function handleClick(event) {
    console.log(event);
  }

  return <button onClick={handleClick}>Click me</button>;
}
```

```jsx
function handleSubmit(e) {
  e.preventDefault();
  console.log("Form submitted");
}
```

```jsx
event.target
event.type
event.preventDefault()
event.stopPropagation()
```

---

### ~={red}11. How does React Handle forms?=~
- React handles forms using controlled components, where the form data is managed by the component's state. This means that the input values are stored in the state and updated through event handlers, allowing for real-time validation and easier management of form data.

In a controlled component:
- Form input values are stored in **state**
- React is the **single source of truth**
- Every change updates state
```jsx
import { useState } from "react";

function LoginForm() {
  const [username, setUsername] = useState("");

  function handleChange(e) {
    setUsername(e.target.value);
  }

  function handleSubmit(e) {
    e.preventDefault();
    console.log(username);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={username}
        onChange={handleChange}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```
---
### ~={red}12. What is hook in React?=~
- React Hooks are functions that allow we to use state and other React features in functional components. 
- <mark style="background: #1D1D1DA6;">Hooks are just functions </mark>
#### 1. ~={red}`useState`=~  : -> `useState` to manage and update data in a component and re-render the UI when that data changes.

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // Initialize state

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```


#### 2. ~={red} `useEffect` =~ : ->  `useEffect`  use in fetching data or manipulating the DOM , update the DOM and Set timers / intervals  and Add event listeners 

```js
useEffect(() => {
  fetch("/api/users")
    .then(res => res.json())
    .then(data => setUsers(data));
}, []);

//-------------------------------------------------------------------------

useEffect(() => {
  const timer = setInterval(() => {
    console.log("Running...");
  }, 1000);

  return () => clearInterval(timer);
}, []);
```

#### 3. ~={red}`useRef`=~ : ->  `useRef` used to store a mutable value that does not reason a re-render when updated.  

it can be use to access a DOM element directly. 

```jsx
function InputFocus() {
  const inputRef = useRef(null);

  return (
    <>
      <input ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>
        Focus Input
      </button>
    </>                             // No re-render
  );
}
```

```jsx
const renderCount = useRef(0);

useEffect(() => {                    // Storing values without re-rendering
  renderCount.current += 1;
});
```
---
#### 4. ~={red}`useMemo`=~ and ~={red}`useCallback`=~ are used to improver performance. 

- ~={red}`useMemo`=~ -> stores the result of a calculation so React doesn't use it so every time the component re-renders.

```jsx
const total = useMemo(() => {
  return price * quantity;
}, [price, quantity]);    // React reuses the saved value
```

- Avoiding recalculating the same value on every render. 

- ~={red}`useCallback`=~  -> stores a function so React doesn't create a new function every render.

```jsx
function Parent() {
  const [count, setCount] = React.useState(0);

  const handleClick = React.useCallback(() => {
    console.log("Button clicked");
  }, []);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        Increase count ({count})
      </button>
      <Child onClick={handleClick} />
    </>
  );
}
```

with dependency means ~={orange}`useCallback(() => {}, []) `=~

```jsx
const handleClick = useCallback(() => {
  console.log(count);
}, [count]);
```

----
### ~={red}13. Explain Reacts Reconciliation algorithm=~
- Updating the real DOM is slow
- React a Virtual DOM means ( copy of the Real DOM )
- Figures out the what changed
- And Updates only those parts in the real DOM. 

- `Reconciliation algorithm` : -> Reconciliation is how React compares the old UI with the new UI and updates only what changed. 
---
### 14. Is ~={blue}`useState`=~ async or sync?
- ~={blue}**`useState`**=~  is asynchronous in behaviour and The updated state is available in the next render. not immediately after calling it.
---
### 15. What is the use of the memo function in React?
- ~={red}**`React.memo`**=~ function prevents unnecessary re-renders of a component by memorizing its output. 