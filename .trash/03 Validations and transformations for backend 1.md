### ~={red} Backend Architecture Layers=~
#### 1.  Repository Layer (Bottom) : 
- Handle Database operations 
	- Database Queries
	- CRUD Operations 
#### 2. Service Layer ( Middleware ): 
- Contains business logic 
	- Sending emails, calling response 
#### 3. Controller Layer (Top)
- Manages HTTP requests/ responses, error code and data formatting. 
---
### **~={red}Three Types of Validations=~**
| ~={pink}Type=~                   | ~={blue}What it checks=~           | ~={blue}Examples from video=~                                                                                                | ~={blue}Purpose=~            |
| -------------------------------- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| **~={orange}Syntactic=~**        | Checking data structure and format | - Is it a valid email? (`user@domain.com`) - Is phone number correctly formatted? - Is date in expected format (YYYY-MM-DD)? | Structure & basic shape      |
| **~={orange}Semantic=~**         | Does the data make logical sense?  | - Date of birth cannot be in the future - Age must be 1–120 - Password & confirm password must match                         | Business/domain rules        |
| **~={orange}Type Validation =~** | Checks data types                  | - Field `age` must be number (not string) - `tags` must be array (not string) - `isActive` must be boolean                   | Prevent type-related crashes |
1. ~={blue}Syntactic=~ 
```js
const express = require('express');
const { z } = require('zod');
const app = express();
app.use(express.json());

const syntacticSchema = z.object({
  email: z.string().email('Invalid email format'),  // Syntactic: Must be valid email structure
  phone: z.string().regex(/^\+\d{10,15}$/, 'Invalid phone format (e.g., +1234567890)'),  // Syntactic: Must match regex
  date: z.string().datetime('Invalid date format (ISO 8601)'),  // Syntactic: Must be valid ISO datetime
});

app.post('/syntactic', (req, res) => {
  try {
    const data = syntacticSchema.parse(req.body);
    res.json({ message: 'Syntactic validation passed', data });
  } catch (error) {
    res.status(400).json({ errors: error.errors.map(e => ({ field: e.path[0], message: e.message })) });
  }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

2. ~={blue}Semantic=~
```js
const express = require('express');
const { z } = require('zod');
const app = express();
app.use(express.json());

const semanticSchema = z.object({
  dob: z.string().datetime().refine(val => new Date(val) <= new Date(), { message: 'DOB cannot be in future' }),  // Semantic: Logical check
  age: z.number().int().min(1, 'Age too low').max(120, 'Age too high'),  // Semantic: Reasonable range
  password: z.string().min(8),
  confirmPassword: z.string(),
}).refine(data => data.password === data.confirmPassword, { message: 'Passwords must match', path: ['confirmPassword'] });  // Semantic: Values match

app.post('/semantic', (req, res) => {
  try {
    const data = semanticSchema.parse(req.body);
    res.json({ message: 'Semantic validation passed', data });
  } catch (error) {
    res.status(400).json({ errors: error.errors.map(e => ({ field: e.path[0], message: e.message })) });
  }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

3. Type 
```js
const express = require('express');
const { z } = require('zod');
const app = express();
app.use(express.json());

const typeSchema = z.object({
  name: z.string('Must be a string'),  // Type: String
  age: z.number('Must be a number'),  // Type: Number
  hobbies: z.array(z.string('Each hobby must be a string')),  // Type: Array of strings
  isActive: z.boolean('Must be a boolean'),  // Type: Boolean
});

app.post('/type', (req, res) => {
  try {
    const data = typeSchema.parse(req.body);
    res.json({ message: 'Type validation passed', data });
  } catch (error) {
    res.status(400).json({ errors: error.errors.map(e => ({ field: e.path[0], message: e.message })) });
  }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

---
### **~={red}Transformations=~**
- Transformations is modify incoming data into a desired format before or after validation
-  Convert query params from string → ~={pink}number (page=“2” → page=2, limit=“20” → limit=20)=~
- Normalize email to lowercase~={pink} User@Gmail. com  -> user@gmail. com =~
- Add country code to phone ~={pink}(“9876543210” → “+919876543210”)=~
- Standardize date format ~={pink}(“5/11/2025” → “2025-11-05”)=~
```js
const express = require('express');
const { z } = require('zod');
const app = express();
app.use(express.json());

const transformSchema = z.object({
  email: z.string().email()  // Validate email first
    .transform((val) => val.toLowerCase()),  // Transform: To lowercase
  phone: z.string()  // Validate string
    .transform((val) => val.startsWith('+') ? val : `+${val}`),  // Transform: Add '+' if missing
  date: z.string().datetime()  // Validate ISO date
    .transform((val) => new Date(val).toISOString().split('T')[0]),  // Transform: To YYYY-MM-DD
  page: z.string()  // Assume from query/body as string
    .transform(Number)  // Transform: String to number
    .pipe(z.number().int().min(1).max(500)),  // Pipe: Further validate range
  limit: z.string()
    .transform(Number)
    .pipe(z.number().int().min(1).max(10000)),
});

app.post('/transform', (req, res) => {
  try {
    const data = transformSchema.parse(req.body);
    res.json({ message: 'Transformations applied successfully', transformedData: data });
  } catch (error) {
    res.status(400).json({ errors: error.errors.map(e => ({ field: e.path[0], message: e.message })) });
  }
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

```json
{                      // Input
  "email": "User@Example.COM",
  "phone": "1234567890",
  "date": "2025-11-05T12:00:00Z",
  "page": "2",
  "limit": "20"
}
```

```json 
{                       // Output
  "message": "Transformations applied successfully",
  "transformedData": {
    "email": "user@example.com",  // Lowercased
    "phone": "+1234567890",       // '+' added
    "date": "2025-11-05",         // Standardized to YYYY-MM-DD
    "page": 2,                    // String to number
    "limit": 20                   // String to number
  }
}              
```
---
### ~={red}Frontend vs. Backend Validation=~
#### 1. Frontend validation.
-  **Purpose**: User experience (UX)
- **Benefit**: Immediate feedback to users

#### 2. Backend Validation 
- **Purpose**: Security and data integrity
- **Mandatory**: Must exist regardless of frontend validation
---
