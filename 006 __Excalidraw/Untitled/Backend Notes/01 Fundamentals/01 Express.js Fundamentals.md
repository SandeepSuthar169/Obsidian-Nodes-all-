### ~={blue}Project Setup =~

```json
bun add -d typescript@latest
```

Use Bun's built-in watcher:
```json
"scripts": {
  "dev": "bun --watch src/server.ts",
  "start": "bun src/server.ts"
}
```

Package.josn
```json
{
  "name": "01-express",
  "module": "src/server.ts",
  "private": true,
  "scripts": {
    "dev": "bun --watch src/server.ts",
    "start": "bun src/server.ts",
    "typecheck": "tsc --noEmit"
  },
  "dependencies": {
    "express": "^5.2.1"
  },
  "devDependencies": {
    "@types/bun": "^1.3.14",
    "@types/express": "^5.0.6",
    "typescript": "^5.9.2"
  }
}
```

tsconfig.json

```json
 {
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "rootDir": "./src",
    "outDir": "./dist",
   "esModuleInterop": true,
   // Environment setup & latest features
    "lib": ["ESNext"],
    "moduleDetection": "force",
    "jsx": "react-jsx",
    "allowJs": true,
    "types": ["bun"],
    // Bundler mode
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "verbatimModuleSyntax": true,
    "noEmit": true,
    // Best practices
    "strict": true,
    "skipLibCheck": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitOverride": true,
    // Some stricter flags (disabled by default)
    "noUnusedLocals": false,
    "noUnusedParameters": false,
    "noPropertyAccessFromIndexSignature": false
  }
}
```

src/app.ts

```ts
import express, { type Request, type Response } from "express";
const app = express()

app.use(express.json())

app.get("/", (req: Request, res: Response) => {
    res.send("Wencome to Express + TypeScript")
})
export default app;
```

src/Server.ts

```ts
import app from "./app";

const POST = 2000;

app.listen(POST, () => {
    console.log(`Servet running on POST: ${POST}`);
})
```

----

Folder Structure 

```json
backend-project/
│
├── src/
│   ├── app.ts
│   ├── server.ts
│   ├── routes/
│   ├── controllers/
│   ├── middleware/
│   ├── models/
│   ├── services/
│   └── utils/
│
├── dist/
│   ├── app.js
│   └── server.js
│
├── node_modules/
├── package.json
├── tsconfig.json
├── .gitignore
└── package-lock.json
```

---
```css
                   +----------------------+
                   |       Client         |
                   | (PUT /books/:id)     |
                   +----------+-----------+
                              |
                              | id, title, author
                              v
                  +-------------------------+
                  |    updateBook API       |
                  |  (Controller Function)  |
                  +-----------+-------------+
                              |
                 Find book by id in books[]
                              |
                +-------------+-------------+
                |                           |
        Book Found?                     Book Not Found
           Yes                               No
            |                                 |
            v                                 v
+--------------------------+      +-------------------------+
| Update title & author    |      | Return 404 Response     |
| book.title = req.body... |      | {success:false,         |
| book.author = req.body...|      |  message:"Book not..." }|
+------------+-------------+      +-------------------------+
             |
             |
             v
 +---------------------------+
 | Updated books[] Array     |
 +------------+--------------+
              |
              |
              v
 +---------------------------+
 | Return 200 Response       |
 | {success:true,            |
 |  data: book,              |
 |  message:"Book Updated"}  |
 +---------------------------+
```

