## ~={blue}Hydration & Caching in Next.js =~

- **Caching is about reusing data/results.**  
- **Hydration is about making server-rendered React interactive in the browser.**
---
### 1. What is Hydration?
- Hydration is when react takes HTML that was already rendered on the server and makes it interactive in the browser. 

```css
Next.js Server
     ↓
HTML generated
     ↓
Browser receives HTML
     ↓
React loads JavaScript
     ↓
React hydrates the HTML
     ↓
Buttons/events become interactive
```

```tsx
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

----
### What is Caching?
- Caching means Next.js can store previously generated/ fetched data so it doesn't have to do the same work repeatedly.

```css
//             A simplified picture:

Request
   ↓
Cache?
 ┌─┴─┐
Yes  No
 ↓    ↓
Data  Fetch
 ↓    ↓
 └──→ Response
```

 Caching can improve:
- performance
- server response time
- scalability
- API/database usage
---
### Controlling Fetch Caching

```tsx
//                 A common TypeScript pattern:
type User = {
  id: number;
  name: string;
  email: string;
};

async function getUsers(): Promise<User[]> {
  const response = await fetch("https://api.example.com/users");

  if (!response.ok) {
    throw new Error("Failed to fetch users");
  }

  return response.json();
}

export default async function UsersPage() {
  const users = await getUsers();

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>
          {user.name} — {user.email}
        </li>
      ))}
    </ul>
  );
}
```

- For a request that should be cached/reused:

```tsx
const response = await fetch("https://api.example.com/users", {
  cache: "force-cache",
});
```

- For data that should always be fresh:

```tsx
const response = await fetch("https://api.example.com/users", {
  cache: "no-store",
});
```

- For time-based revalidation:

```tsx
const response = await fetch("https://api.example.com/users", {
  next: {
    revalidate: 60,
  },
});
```
---

### Hydration + Caching Together

```tsx
export default async function ProductPage() {
  const response = await fetch(
    "https://api.example.com/products/1",
    {
      next: {
        revalidate: 300,
      },
    }
  );

  const product = await response.json();

  return (
    <main>
      <h1>{product.name}</h1>
      <p>{product.price}</p>
    </main>
  );
}
```

```css
Browser
   ↓
Request /products/1
   ↓
Next.js Server
   ↓
Check/fetch cached data
   ↓
Render HTML
   ↓
Browser receives HTML
   ↓
Client Components hydrate
   ↓
Interactive UI
```

----
### Hydration Error
- Hydration error happens when the HTML React renders in on the server doesn't match what React expects to render on the client. 
```css
Server HTML
    ↓
Browser receives it
    ↓
React hydrates it
    ↓
❌ Server output !== Client output
    ↓
Hydration Error
```

---

- Common example

```tsx
//             This can cause a hydration error:

export default function Page() {
  return (
    <div>
      <p>{new Date().toLocaleTimeString()}</p>
    </div>
  );
}
```

Why?
- The server might render:

```
10:30:01 AM
```

- while the browser renders:

```
10:30:03 AM
```

React sees different HTML and complains.

#### 7. The most common causes

| ~={2}Cause=~        | ~={2}Example=~                          |
| ------------------- | --------------------------------------- |
| Current time        | `new Date()`                            |
| Random values       | `Math.random()`                         |
| Browser API         | `window`, `document`                    |
| Storage             | `localStorage`                          |
| Different data      | Server vs client API results            |
| Browser extensions  | Modified HTML                           |
| Invalid HTML        | Incorrect element nesting               |
| Environment checks  | `typeof window !== "undefined"`         |
| Different rendering | Server and client produce different JSX |
