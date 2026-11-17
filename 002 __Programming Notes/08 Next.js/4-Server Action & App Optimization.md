## ~={4}Server Action & App Optimization=~

### ~={3}What are Server Actions?=~
- Server Actin are asynchronous function that run only on the server and can be called directly from React Component (Client, Server Component).
- They allow you to execute server-side logic such as database operations authentication, and from handling without creating API routes.
- A server action is a server-side function marked with "use server" that can be invoked from React components.

```
Client                              
   ↓                                    
API Route                  
   ↓                       
Database                  
```

you can now do:
```
Client
   ↓
Server Action
   ↓
Database
```

#### ~={4}Server Action Decision Table=~

| 1. Form Submmission                 | ~={3}`YES`=~ |
| ----------------------------------- | ------------ |
| 2. Create/ Update/ Delete Data      | ~={3}`YES`=~ |
| 3. Internal UI Mutations            | ~={3}`YES`=~ |
| 4. Public API                       | ~={1}`NO`=~  |
| 5. Webhooks -> Stripe,GitHub,PayPal | ~={1}`NO`=~  |
| 6. Real-time Features               | ~={1}`NO`=~  |
| 7. External Clients                 | ~={1}`NO`=~  |
| 8. Heavy Backend logic              | ~={1}`NO`=~  |
| 9. Live Search                      | ~={1}`NO`=~  |

---
#### ~={3}1. Creating Data=~

Example: Create a user

```tsx
<form action={createUser}>
```

```tsx
"use server";

export async function createUser(formData: FormData) {
  // Save to database
}
```

Good for:
- User registration
- Creating posts
- Adding comments
- Creating orders
---
#### ~={3}2. Updating Data=~

Example:

```tsx
<button onClick={() => updateProfile(id)}>
```

```tsx
"use server";

export async function updateProfile(id: string) {
  // Update database
}
```

Good for:
- Edit profile
- Update product
- Change password
- Update settings
---
#### ~={3}3. Deleting Data=~

```tsx
"use server";

export async function deletePost(id: string) {
   await prisma.post.delete({
      where: { id }
   });
}
```

---
#### ~={3}4. Processing Forms=~

Instead of
```tsx
onSubmit -> fetch("/api") -> backend
```

Use
```tsx
<form action={createPost}>
```

---
#### ~={3}5. Accessing Sensitive Data=~

Need
- Database
- Secret API keys
- Environment variables
- Filesystem
- Never expose secrets to the browser.

```tsx
"use server";

const apiKey = process.env.OPENAI_API_KEY;
```
---
#### ~={3}6. Authentication & Authorization=~

```tsx
"use server";

const session = await auth();

if (!session) {
    throw new Error("Unauthorized");
}
```

Authentication checks should always happen on the server.

---
#### ~={3}Server Action vs Server Component=~

| Task                 | Use              |
| -------------------- | ---------------- |
| Display products     | Server Component |
| Display dashboard    | Server Component |
| Display user profile | Server Component |
| Create product       | Server Action    |
| Update profile       | Server Action    |
| Delete comment       | Server Action    |

---

A Simple Decision Flow
```
Does it change data?
        │
     Yes ─────────► Server Action
        │
        No
        │
Is it loading data for a page?
        │
     Yes ─────────► Server Component
        │
        No
        │
Is it for external services, mobile apps, or webhooks?
        │
     Yes ─────────► API Route
        │
        No
        │
Is it browser-only (localStorage, camera, etc.)?
        │
     Yes ─────────► Client Component
```

---
### Quick rule to remember
- **Server Component** → Read data.
- **Server Action** → Write/change data.
- **API Route** → Expose endpoints to external consumers or handle webhooks.
- **Client Component** → Handle UI state, user interactions, and browser APIs.