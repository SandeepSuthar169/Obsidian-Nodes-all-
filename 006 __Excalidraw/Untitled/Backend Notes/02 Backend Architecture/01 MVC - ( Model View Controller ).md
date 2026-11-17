## MVC - ( Model - View - Controller )
### 1. Model handle data
-  The model is a application's data and contains the logic for interacting with the database. 
- Model = Data + database related logic

### 2. Controller - handles requests
- The Controller receives the request, decides what should happen, calls the appropriate model/ service, and sends the response
- Controller = Handles HTTP request and response 

### 3. View - presents the data
- The view generates what the user sees, such as HTML

```
Frontend
   ↓
Controller
   ↓
Model
   ↓
Database
   ↑
Controller
   ↓
JSON response
   ↓
Frontend
```

Model = What data?  
Controller = What should happen?  
View = What should be shown?

---

```ts                   
//                       src/models/todo.model.ts
import { pool } from "../config/db";

export interface Todo {
  id: number;
  title: string;
  description: string | null;
  completed: boolean;
  created_at: Date;
  updated_at: Date;
}

export interface CreateTodoData {
  title: string;
  description?: string;
}

export interface UpdateTodoData {
  title?: string;
  description?: string;
  completed?: boolean;
}

// GET all todos
export const getAllTodos = async (): Promise<Todo[]> => {
  const result = await pool.query(
    `SELECT *
     FROM todos
     ORDER BY created_at DESC`
  );

  return result.rows;
};

// GET todo by ID
export const getTodoById = async (
  id: number
): Promise<Todo | null> => {
  const result = await pool.query(
    `SELECT *
     FROM todos
     WHERE id = $1`,
    [id]
  );

  return result.rows[0] || null;
};

// CREATE todo
export const createTodo = async (
  data: CreateTodoData
): Promise<Todo> => {
  const result = await pool.query(
    `INSERT INTO todos (title, description)
     VALUES ($1, $2)
     RETURNING *`,
    [data.title, data.description ?? null]
  );

  return result.rows[0];
};

// UPDATE todo
export const updateTodo = async (
  id: number,
  data: UpdateTodoData
): Promise<Todo | null> => {
  const result = await pool.query(
    `UPDATE todos
     SET
       title = COALESCE($1, title),
       description = COALESCE($2, description),
       completed = COALESCE($3, completed),
       updated_at = CURRENT_TIMESTAMP
     WHERE id = $4
     RETURNING *`,
    [
      data.title,
      data.description,
      data.completed,
      id,
    ]
  );

  return result.rows[0] || null;
};

// DELETE todo
export const deleteTodo = async (
  id: number
): Promise<Todo | null> => {
  const result = await pool.query(
    `DELETE FROM todos
     WHERE id = $1
     RETURNING *`,
    [id]
  );

  return result.rows[0] || null;
};
```

```ts
//               src/controllers/todo.controller.ts
import { Request, Response } from "express";

import {
  getAllTodos,
  getTodoById,
  createTodo,
  updateTodo,
  deleteTodo,
} from "../models/todo.model";

// GET /api/todos
export const getTodos = async (
  req: Request,
  res: Response
) => {
  try {
    const todos = await getAllTodos();

    res.status(200).json({
      success: true,
      data: todos,
    });
  } catch (error) {
    console.error(error);

    res.status(500).json({
      success: false,
      message: "Failed to fetch todos",
    });
  }
};

// GET /api/todos/:id
export const getTodo = async (
  req: Request,
  res: Response
) => {
  try {
    const id = Number(req.params.id);

    if (Number.isNaN(id)) {
      return res.status(400).json({
        success: false,
        message: "Invalid todo ID",
      });
    }

    const todo = await getTodoById(id);

    if (!todo) {
      return res.status(404).json({
        success: false,
        message: "Todo not found",
      });
    }

    res.status(200).json({
      success: true,
      data: todo,
    });
  } catch (error) {
    console.error(error);

    res.status(500).json({
      success: false,
      message: "Failed to fetch todo",
    });
  }
};

// POST /api/todos
export const addTodo = async (
  req: Request,
  res: Response
) => {
  try {
    const { title, description } = req.body;

    if (!title || typeof title !== "string") {
      return res.status(400).json({
        success: false,
        message: "Title is required",
      });
    }

    const todo = await createTodo({
      title,
      description,
    });

    res.status(201).json({
      success: true,
      data: todo,
    });
  } catch (error) {
    console.error(error);

    res.status(500).json({
      success: false,
      message: "Failed to create todo",
    });
  }
};

// PATCH /api/todos/:id
export const editTodo = async (
  req: Request,
  res: Response
) => {
  try {
    const id = Number(req.params.id);

    if (Number.isNaN(id)) {
      return res.status(400).json({
        success: false,
        message: "Invalid todo ID",
      });
    }

    const { title, description, completed } = req.body;

    const todo = await updateTodo(id, {
      title,
      description,
      completed,
    });

    if (!todo) {
      return res.status(404).json({
        success: false,
        message: "Todo not found",
      });
    }

    res.status(200).json({
      success: true,
      data: todo,
    });
  } catch (error) {
    console.error(error);

    res.status(500).json({
      success: false,
      message: "Failed to update todo",
    });
  }
};

// DELETE /api/todos/:id
export const removeTodo = async (
  req: Request,
  res: Response
) => {
  try {
    const id = Number(req.params.id);

    if (Number.isNaN(id)) {
      return res.status(400).json({
        success: false,
        message: "Invalid todo ID",
      });
    }

    const todo = await deleteTodo(id);

    if (!todo) {
      return res.status(404).json({
        success: false,
        message: "Todo not found",
      });
    }

    res.status(200).json({
      success: true,
      message: "Todo deleted successfully",
    });
  } catch (error) {
    console.error(error);

    res.status(500).json({
      success: false,
      message: "Failed to delete todo",
    });
  }
};
```

```ts 
//                      src/routes/todo.routes.ts
import { Router } from "express";

import {
  getTodos,
  getTodo,
  addTodo,
  editTodo,
  removeTodo,
} from "../controllers/todo.controller";

const router = Router();

router.get("/", getTodos);

router.get("/:id", getTodo);

router.post("/", addTodo);

router.patch("/:id", editTodo);

router.delete("/:id", removeTodo);

export default router;
```

---

                    HTTP Request
                         │
                         ▼
                 ┌──────────────┐
                 │    ROUTES    │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │  CONTROLLER  │
                 │              │
                 │ HTTP logic   │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │    MODEL     │
                 │              │
                 │ SQL queries  │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │  PostgreSQL  │
                 │    Neon      │
                 └──────────────┘


