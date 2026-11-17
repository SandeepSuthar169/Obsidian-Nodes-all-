## Modular architecture ?
- Modular architecture organizes backend by business feature/ module, rather than putting every controller.

```
src/
├── controllers/
│   ├── todo.controller.ts
│   └── user.controller.ts
├── services/
│   ├── todo.service.ts
│   └── user.service.ts
├── repositories/
│   ├── todo.repository.ts
│   └── user.repository.ts
└── routes/
    ├── todo.routes.ts
    └── user.routes.ts
```

```
src/
├── modules/
│   ├── todos/
│   │   ├── todo.controller.ts
│   │   ├── todo.service.ts
│   │   ├── todo.repository.ts
│   │   ├── todo.routes.ts
│   │   └── todo.types.ts
│   │
│   └── users/
│       ├── user.controller.ts
│       ├── user.service.ts
│       ├── user.repository.ts
│       ├── user.routes.ts
│       └── user.types.ts
│
├── config/
│   └── db.ts
│
├── middleware/
│   └── error.middleware.ts
│
├── app.ts
└── server.ts
```

```
modules/
└── todos/
    ├── todo.controller.ts
    ├── todo.service.ts
    ├── todo.repository.ts
    ├── todo.routes.ts
    └── todo.types.ts
```

```
modules/
├── auth/
├── users/
├── todos/
├── comments/
├── notifications/
├── payments/
└── admin/
```

