## Service layer Architecture 
- A Server Layer is a between the controller and models.
- It is job is to contain business logic so you controllers don't become huge.

| Layer                | Responsibility                |
| -------------------- | ----------------------------- |
| **Route**            | Maps URL → controller         |
| **Controller**       | Handles HTTP request/response |
| **Service**          | Business logic                |
| **Model/Repository** | SQL/database operations       |
| **Database**         | Stores data                   |

 ----
 Controller

```ts
export const addTodo = async (
  req: Request,
  res: Response
) => {
  try {
    const todo = await todoService.createTodo(req.body);

    res.status(201).json({
      success: true,
      data: todo
    });
  } catch (error) {
    // handle error
  }
};
```

Service

```ts
export const createTodo = async (
  data: CreateTodoData
) => {

  if (!data.title || data.title.trim().length === 0) {
    throw new Error("Title is required");
  }

  const todo = await todoModel.createTodo({
    title: data.title.trim(),
    description: data.description
  });

  return todo;
};
```

Model

```ts
export const createTodo = async (
  data: CreateTodoData
) => {
  const result = await pool.query(
    `INSERT INTO todos (title, description)
     VALUES ($1, $2)
     RETURNING *`,
    [
      data.title,
      data.description ?? null
    ]
  );

  return result.rows[0];
};
```

```
todo-api/
│
├── src/
│   │
│   ├── config/
│   │   └── db.ts
│   │
│   ├── controllers/
│   │   └── todo.controller.ts
│   │
│   ├── services/
│   │   └── todo.service.ts
│   │
│   ├── models/
│   │   └── todo.model.ts
│   │
│   ├── routes/
│   │   └── todo.routes.ts
│   │
│   ├── middleware/
│   │   └── error.middleware.ts
│   │
│   ├── types/
│   │   └── todo.types.ts
│   │
│   ├── app.ts
│   └── server.ts
│
├── .env
├── package.json
└── tsconfig.json
```

Controller = HTTP logic
Service = application/business logic  
Model = database logic

```
Controller
    │
    │ req.body
    ▼
Service
    │
    │ "Can this user create this Todo?"
    │ "Is the title valid?"
    │ "Does this Todo already exist?"
    ▼
Model
    │
    │ SQL
    ▼
PostgreSQL
```