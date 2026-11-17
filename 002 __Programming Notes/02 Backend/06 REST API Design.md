### ~={red}URL Structure Best Practices=~
```json
https://api.example.com/v1/organizations/123/tasks?status=active&limit=10
\___/   \_/ \_________/\_/\_____________/\_/\___________________________/
  |      |       |      |        |        |           |
Scheme  Subdomain Domain  Version  Resource  ID       Query Params
```

---
### ~={red}REST Constraints (The 5 Architectural Constraints)=~

#### 1.  ~={orange}Client-Server Architecture=~
-  Client-Server architecture is a model where multiple client (users) connect to a centralized server to access data services.  
#### 2.  ~={orange}Uniform Interface: Standardized Communication (REST)=~
- architecture design a standardized way from clients and server to communicate of the application. 

#### 3.  ~={orange}Layered System: Hierarchical Layers (REST)=~. 
- The system is organized into multiple hierarchical layers, where each layer has a specific responsibility and communicates. 
```txt
Client
   ↓
Load Balancer
   ↓
API Gateway
   ↓
Application Server
   ↓
Database Server
```



#### 4. ~={orange}CACHEABLE=~
- Cacheable is a REST constraint that requires server responses to explicitly define whether they can cached and for how long, improving performance and scalability.
	- ✅ Performance
	- ✅ Response time
	- ✅ Server efficiency

If cached → Response is returned immediately  
If not cached → Server fetches from DB and stores it

```js
const express = require('express');
const app = express();

// Example product data (simulating database)
const products = [
    { id: 1, name: "Laptop" },
    { id: 2, name: "Mobile" }
];

app.get('/products', (req, res) => {
    // Cache for 5 minutes (300 seconds)
    res.set('Cache-Control', 'public, max-age=300');
    res.json(products);
});

app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```

```txt
Client
   ↓
   
Nginx (Reverse proxy caching) / 
Redis (Distributed caching) /
CDN (for static assets) 
   
   ↓
Node.js Server
   ↓
Database
```

#### 5. ~={orange}Stateless vs Stateful=~

| Stateless                    | Stateful                         |
| ---------------------------- | -------------------------------- |
| Server stores no client data | Server stores session data       |
| Each request is independent  | Requests depend on previous ones |
| Scalable                     | Harder to scale                  |
| REST principle               | Traditional web apps             |

----
### ~={red}Designing REST APIs - The Core Concepts=~
#### 1.  Always use PLURAL nouns for collection
```js
// ✅ CORRECT
GET    /api/projects        // Get all projects by d
GET    /api/projects/123    // Get project with ID 123
POST   /api/projects        // Create new project
PATCH    /api/projects/123    // Update project 123
DELETE /api/projects/123    // Delete project 123

// ❌ WRONG
GET /api/project           // Don't use singular
GET /api/getProjects       // Don't use verbs
POST /api/createProject    // Don't use verbs
```

```TS
GET    /api/projects        // Get all projects 

// By Default with in URL page = 1, limit = 10, sort = "createdAt" [desc]
//            - sortby - some category [name, status]
//            - sortOrder - asc, desc 
//            - Filter of List e.g. -> name = org4, stauts = active
IMP
// If List is empty and array is empty don't send 404 status not found, alwaly send send 200 OK  If list is empty or not 
---------------------------------------------------------------------------------
PATCH  /api/projects/123    // Update project 123

// PATCH use for in updaing fildes 

GET | PATCH | DELETE     //-> getting, updating, deleting a single organaization all the URL almost some & methos will be different. 

---------------------------------------------------------------------------------
DELETE  
// When use DELETE methos don't send response.
// Only send status code 204 (No Content)

---------------------------------------------------------------------------------


```

#### 2. Custom Action
1. Archive Action
- Archive is hiding a record without deleting it. The data stays in database, but it disappears from normal lists and we can restore it later

```ts
PATCH  /api/events/:id/archive       → archive event 42
PATCH  /api/events/:id/unarchive     → restore event 42
GET    /api/events                   → only non-archived events (default)
GET    /api/events/archived          → only archived events
GET    /api/events?archived=true     → alternative to the line above
```

2. Clone Action 
- Clone ( duplicate ) crates a new record by copying an existing one. The original stays untouched. 

|                 | Create                | Clone                        |
| --------------- | --------------------- | ---------------------------- |
| Client sends    | Full data in the body | Only the source `id`         |
| Data comes from | The user's input      | Existing record              |
| Result          | New record            | New record (copy of another) |
```ts
POST /api/v1/todos/42/clone                        → copy todo 42
POST /api/v1/todos/42/clone  { "title": "Sprint 2" }  → copy with an overridden title
```

##### Use Cases

| Use case                 | What gets cloned                                                       |
| ------------------------ | ---------------------------------------------------------------------- |
| **Templates**            | Copy a project, invoice or form as a starting point                    |
| **Recurring work**       | Duplicate last week’s task or event with small changes                 |
| **E-commerce**           | Clone a product to make a variant (same details, different size/color) |
| **Notion / Google Docs** | “Make a copy”                                                          |
| **CMS**                  | Duplicate a blog post as a new draft                                   |
| **Safe experimenting**   | Edit the copy without touching the original                            |
```ts
// controllers/todo.controller.ts
import { Request, Response } from "express";
import { z } from "zod";
import pool from "../config/db";
import { asyncHandler } from "../utils/asyncHandler";
import { AppError } from "../utils/AppError";
import logger from "../utils/logger";

const cloneParamsSchema = z.object({
  id: z.coerce.number().int().positive(),
});

// optional overrides for the copy
const cloneBodySchema = z
  .object({
    title: z.string().trim().min(1).max(200).optional(),
  })
  .optional()
  .default({});

// POST /api/v1/todos/:id/clone
export const cloneTodo = asyncHandler(async (req: Request, res: Response) => {
  const params = cloneParamsSchema.safeParse(req.params);
  if (!params.success) throw new AppError("Invalid todo id", 400);

  const body = cloneBodySchema.safeParse(req.body);
  if (!body.success) throw new AppError("Invalid request body", 400);

  const { id } = params.data;
  const { title } = body.data;

  const { rows } = await pool.query<Todo>(
    `INSERT INTO todos (title, description, "isComplete", "createdAt")
     SELECT
       COALESCE($2, 'Copy of ' || title),
       description,
       false,
       NOW()
     FROM todos
     WHERE id = $1
     RETURNING id, title, description, "isComplete", "createdAt"`,
    [id, title ?? null]
  );

  if (rows.length === 0) throw new AppError("Todo not found", 404);

  logger.info("Todo cloned", { sourceId: id, newId: rows[0].id });

  res.status(201).json({
    success: true,
    message: "Todo cloned successfully",
    data: rows[0],
  });
});
```

---
#### 3. HTTP Methods & Idempotent and HTTP Status Codes
##### What is Idempotent?
- Making the same request multiple times = Same result

| Method   | Idempotent | Purpose                          | Status Codes                        |
| -------- | ---------- | -------------------------------- | ----------------------------------- |
| `GET`    | ✅ Yes      | Retrieve data                    | 200 (OK)                            |
| `POST`   | ❌ No       | Create resource / Custom actions | 201 (Created), 200 (Custom actions) |
| `PUT`    | ✅ Yes      | Complete replacement of resource | 200 (OK)                            |
| `PATCH`  | ✅ Yes      | Partial update of resource       | 200 (OK)                            |
| `DELETE` | ✅ Yes      | Remove resource                  | 204 (No Content)                    |

| Code | Meaning             | When to Use                    |
| ---- | ------------------- | ------------------------------ |
| 200  | OK                  | Successful GET, PATCH          |
| 201  | Created             | Successful POST (new resource) |
| 204  | No Content          | Successful DELETE              |
| 400  | Bad Request         | Invalid input                  |
| 401  | Unauthorized        | Not authenticated              |
| 403  | Forbidden           | Not authorized                 |
| 404  | Not found           | Resource not found             |
| 409  | Conflict            | Duplicate/conflict             |
| 500  | Server Error        | Something broke                |
| 503  | Service unavailable | Server down                    |

#### 3. Pagination, Sorting & Filtering
```js
const getAllOrganizations = async (req, res) => {
    try {
        // 📌 PAGINATION - Default values if not provided
        const page = parseInt(req.query.page) || 1;
        const limit = parseInt(req.query.limit) || 10;
        const skip = (page - 1) * limit;

        // 📌 SORTING - Default sort by createdAt descending
        const sortBy = req.query.sortBy || 'createdAt';
        const sortOrder = req.query.sortOrder || 'desc';
        const sort = { [sortBy]: sortOrder === 'asc' ? 1 : -1 };

        // 📌 FILTERING
        const filter = {};
        if (req.query.status) {
            filter.status = req.query.status;
        }

        // Fetch data
        const data = await Organization.find(filter)
            .sort(sort)
            .skip(skip)
            .limit(limit);

        // Get total count for pagination metadata
        const total = await Organization.countDocuments(filter);
        const totalPages = Math.ceil(total / limit);

        // ✅ Standard paginated response
        res.status(200).json({
            data,
            total,
            page,
            totalPages
        });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
};
```

#### 5 Custom Actions (Non-CRUD)
- not plain CRUD — it's a business operation.
- Custom actions are endpoints that represent business operations, not simple data manipulation. 
	- Approve a user
	- Cancel an order
	- Reset a password
	- Send an email
	- Activate an account
	- Upload a file
	- Verify OTP

#### 6. Consistency & Best Practices
```js
// 1. Use camelCase for JSON fields
{
    "createdAt": "2024-01-01",
    "userName": "john",
    "isActive": true
}

// 2. Provide sensible defaults
// If client doesn't send 'status', default to 'active'
const status = req.body.status || 'active';

// 3. Be consistent across all resources
// All resources use: limit, page, sortBy, sortOrder
// All resources return: data, total, page, totalPages

// 4. Use interactive documentation (Swagger/OpenAPI)
// 5. Make APIs intuitive and predictable
```
----
