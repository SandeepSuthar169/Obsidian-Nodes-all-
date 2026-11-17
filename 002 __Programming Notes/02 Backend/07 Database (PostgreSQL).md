
### ~={blue}Persistence in Database=~
Persistence in a database is ability of a database to store data permanently, so that the data remains saved even after the application is closed. 

~={orange}Importance =~
- Permanent Storage
- Data recovery 
- Reliability 
- Consistency

~={orange}Types=~
- File-based persistence -> text files, CSV, JSON.
- Database persistence   -> MySQL, PostgreSQL, MongoDB.
- Cloud persistence      -> Google, Oracle, Meta.
---
### ~={blue}Database=~ 
A any kind of structured Storage can be considered a database


---
## ~={blue}PostgreSQL=~

~={red}1. Data Types Demo=~

```sql
 CREATE TABLE data_types_demo (
    id SERIAL PRIMARY KEY,         // Identify row or collums  +1
    some_smallint SMALLINT,        // SMALLINT < INTEGER < BIGINT (Capacity)
    some_integer INTEGER,          //   
    some_bigint BIGINT,
    
    some_decimal DECIMAL(10, 2),   //  10 -> max length , 2 -> .11 
		- When comes accuracy go to the DECIMAL 
		- DECIMAL is Slow 
		- Ex -> Price  -> [ 99.99 to 99.99 ] strick 
    
    some_numeric NUMERIC(10, 2),   //  10 -> max length , 2 -> .11 
		- When comes accuracy go to the DECIMAL 
		- DECIMAL is Fast
		- Ex -> Size -> [ 55.899 to 55.90 or 56 ] not strick 
 		  
	
    some_real REAL,
    some_double DOUBLE PRECISION,
    
    some_char CHAR(10),       // mostly dont use 
		- It is Fix charecter lenght (10)
		- Ex -> "asdfghjkly", "asd_______",
	
    some_varchar VARCHAR(255),   // best for MySQL  // 255 is random number
	    - It is Max charecter length (255)
	      Ex -> "asdf", "wejrwrwr", "q"
    
    some_text TEXT,          // Always Go there PostgreSQL says in documentaion
	    - It is not fix length of inputs any length (no upper limit )
	    - Best for PostgreSQL
		
    some_boolean BOOLEAN,          // true | false
    some_date DATE,
    some_time TIME,                // hr:min:sec
    some_timestamp TIMESTAMP,      // Date + Time
    some_timestamptz TIMESTAMPTZ,  // Store with time zone 
    some_interval INTERVAL,        // 10 Days  | 1 Week | 2 Months
    some_uuid UUID,                // Use for Table Primiry Key   
    some_json JSON,                // JSON format
    some_jsonb JSONB,              // JSONB is JSON Binnary native for PostgreSQL
		- for Performace Reason | Most use it in PostgreSQL   
    some_array INTEGER[],  | TEXT[]   // it is a Array with datatype 
    some_inet INET,
    some_macaddr MACADDR,
    some_point POINT,
    some_xml XML
);
```

```sql
( 
 32767,                                     -- SMALLINT 
 2147483647,                                -- INTEGER 
 9223372036854775807,                       -- BIGINT 
 1234.56,                                   -- DECIMAL 
 9876.54,                                   -- NUMERIC 
 123.456,                                   -- REAL 
 123.4567890123456,                         -- DOUBLE PRECISION 
 'CHAR10PAD ',                              -- CHAR(10) 
 'Variable length string',                  -- VARCHAR(255) 
 'Long text content can go here',           -- TEXT 
 TRUE,                                      -- BOOLEAN 
 '2024-03-14',                              -- DATE 
 '15:30:00',                                -- TIME 
 '2024-03-14 15:30:00',                     -- TIMESTAMP 
 '2024-03-14 15:30:00+00',                  -- TIMESTAMPTZ 
 '2 years 3 months 4 days',                 -- INTERVAL 
 'a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11',    -- UUID 
 '{"name": "John", "age": 30}',             -- JSON 
 '{"name": "John", "age": 30}',             -- JSONB 
 ARRAY[1, 2, 3, 4, 5],                      -- INTEGER[] 
 '192.168.1.1',                             -- INET 
 '08:00:2b:01:02:03',                       -- MACADDR 
 '(10,20)',                                 -- POINT 
 '<root><element>Some XML element</root>'   -- XML 82 
 );
```

---

```sql
-- migrate:up

-- Create custom enum types
CREATE TYPE project_status AS ENUM ('active', 'completed', 'archived');
CREATE TYPE task_status AS ENUM ('pending', 'in_progress', 'completed', 'cancelled');
CREATE TYPE member_role AS ENUM ('owner', 'admin', 'member');

-- Create users table
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(), -- 30f46a36-c958-47a6-bf75-51df55f45ac0
    email TEXT NOT NULL UNIQUE,
    full_name TEXT NOT NULL,
    password_hash TEXT NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create user_profiles table (one-to-one with users)
CREATE TABLE user_profiles (
    user_id UUID PRIMARY KEY REFERENCES users ON DELETE CASCADE,
    avatar_url TEXT,
    bio TEXT,
    phone TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

- must be name will be snack_case or camel_case 

```SQL

-- Create projects table
CREATE TABLE projects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    description TEXT,
    status project_status NOT NULL DEFAULT 'active',
    owner_id UUID NOT NULL REFERENCES users ON DELETE RESTRICT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Create tasks table (one-to-many with projects)
CREATE TABLE tasks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_id UUID NOT NULL REFERENCES projects ON DELETE CASCADE,
    title TEXT NOT NULL,
    description TEXT,
    priority INTEGER NOT NULL DEFAULT 1 CHECK (priority BETWEEN 1 AND 5),
    status task_status NOT NULL DEFAULT 'pending',
    due_date DATE,
    assigned_to UUID REFERENCES users ON DELETE SET NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);


-- Create project_members table (many-to-many between projects and users)
CREATE TABLE project_members (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_id UUID NOT NULL REFERENCES projects ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES users ON DELETE CASCADE,
    role member_role NOT NULL DEFAULT 'member',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (project_id, user_id)
);

-- migrate:down

-- Drop tables
DROP TABLE IF EXISTS project_members;
DROP TABLE IF EXISTS tasks;
DROP TABLE IF EXISTS projects;
DROP TABLE IF EXISTS user_profiles;
DROP TABLE IF EXISTS users;

-- Drop enum types
DROP TYPE IF EXISTS member_role;
DROP TYPE IF EXISTS task_status;
DROP TYPE IF EXISTS project_status;
```

---

Seeding

```sql
-- migrate:up

-- Seed users first and store their IDs in variables
WITH inserted_users AS (
    INSERT INTO users (email, full_name, password_hash) VALUES
        ('john@example.com', 'John Doe', 'hash1'),
        ('jane@example.com', 'Jane Smith', 'hash2'),
        ('bob@example.com', 'Bob Wilson', 'hash3'),
        ('alice@example.com', 'Alice Brown', 'hash4')
    RETURNING id, email
),
-- Seed user_profiles
inserted_profiles AS (
    INSERT INTO user_profiles (user_id, avatar_url, bio, phone)
    SELECT
        id,
        'https://example.com/avatar' || row_number() OVER () || '.jpg',
        CASE
            WHEN email LIKE 'john%' THEN 'Project Manager with 5 years experience'
            WHEN email LIKE 'jane%' THEN 'Senior Developer'
            WHEN email LIKE 'bob%' THEN 'UX Designer'
            ELSE 'Business Analyst'
        END,
        '+123456789' || row_number() OVER ()
    FROM inserted_users
)
```