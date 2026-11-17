## ~={blue}1. Caching in Next.js=~
- Caching  =  Storing the result of a data request and reusing it instead of fetching again.

#### In Next.js
- Data fetched using ~={2}`fetch()`=~ can be stored in a data cache. 
- Future requests can reuse that cached data instead of calling the API again.

Purpose:
- Improve performance
- Reduce API calls
- Reduce server load

---
## ~={blue}2. How Next.js Handles caching=~
Next.js extends the native `fetch()` API:
It allows you to control caching and revalidation per request

There are 3 main behaviours.
- No Caching (Fresh data)
- Full Caching
- Cached with revalidation

```ts
async function getUsers() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users"
  );
  
  return response.json();
}
```

---
## ~={blue}3. Default Behaviours=~
- By default - NOT cached
- Every request hits the API again

---
## ~={blue}4. Caching with fetch()=~
##### 4.1 Data Cache
- Next.js automatically cached `fetch()` requests.
- The data is stored in the Data Cache.
- Future requests can reuse it

```ts
async function getPosts() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/posts"
  );

  return response.json();
}
```

##### 4.1 Force Caching
- First request -> API call
- Data stored in cache
- Next requests -> data returned from cache

```tsx
async function getProducts() {
  const response = await fetch(
    "https://api.example.com/products",{
      cache: "force-cache"
    });
    
  const data = await res.json()
 

  return <div>{data.length}</div>
}
```

---
4.2 Disable Caching 
- Sometimes you always want fresh data.

Examples:
- Stock prices
- Weather updates
- Live sports scores

```ts
async function getStockPrices() {
  const response = await fetch(
    "https://api.example.com/stocks", {
      cache: "no-store"
    });

  return response.json();
}
```

---

4.3 Time-Based Revalidation
Revalidation means updating cached data after a specific amount of time.
- 0-3600 seconds → Cached data
- 3601 seconds → Refresh cache
```ts
async function getArticles() {
  const response = await fetch(
    "https://api.example.com/articles",{
      next: {
        revalidate: 3600
      }}
  );

  return response.json();
}
```

---

4.4 Route-Level Revalidation
- You can configure an entire page.
- Cache the entire route for 60 seconds.

```tsx
export const revalidate = 60;

async function getPosts() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/posts"
  );

  return response.json();
}

export default async function PostsPage() {
  const posts = await getPosts();

  return (
    <div>
      {posts.map((post: any) => (
        <h2 key={post.id}>{post.title}</h2>
      ))}
    </div>
  );
}
```

---
4.4 Route-Level Revalidation
 - On-Demand Revalidation means updating the cache immediately when something changes, instead of waiting for a timer.

```
app
│
├── posts
│   └── page.tsx
│
├── actions
│   └── postActions.ts
│
└── components
    └── AddPost.tsx
```

```
User opens /posts

↓

Posts are cached

↓

Admin adds a new post

↓

revalidatePath("/posts")

↓

Cache is cleared

↓

The next request creates a fresh cache
```

lib/database.ts

```tsx
export interface Post {
  id: number;
  title: string;
}

export let posts: Post[] = [
  {
    id: 1,
    title: "Next.js"
  },
  {
    id: 2,
    title: "React"
  }
];

export async function getPosts(): Promise<Post[]> {
  return posts;
}

export async function addPost(
  title: string
): Promise<void> {
  posts.push({
    id: posts.length + 1,
    title
  });
}
```

**`app/posts/page.tsx`**

```ts
export const revalidate = 3600;

import { getPosts } from "@/lib/database";

export default async function PostsPage() {
  const posts = await getPosts();

  return (
    <div>
      <h1>Posts</h1>

      {posts.map((post) => (
        <div key={post.id}>
          {post.title}
        </div>
      ))}
    </div>
  );
}
```

app/actions/postActions.ts

```ts
"use server";

import { addPost } from "@/lib/database";
import { revalidatePath } from "next/cache";

export async function createPost(
  formData: FormData
) {
  const title = formData.get(
    "title"
  ) as string;

  await addPost(title);

  revalidatePath("/posts");
}
```

app/components/AddPost.tsx
```tsx
import { createPost } from "../actions/postActions";

export default function AddPost() {
  return (
    <form action={createPost}>
      <input
        type="text"
        name="title"
        placeholder="Post title"
      />

      <button type="submit">
        Add Post
      </button>
    </form>
  );
}
```

app/posts/page.tsx

```tsx
export const revalidate = 3600;

import AddPost from "../components/AddPost";
import { getPosts } from "@/lib/database";

export default async function PostsPage() {
  const posts = await getPosts();

  return (
    <div>
      <h1>Posts</h1>

      <AddPost />

      {posts.map((post) => (
        <div key={post.id}>
          {post.title}
        </div>
      ))}
    </div>
  );
}
```