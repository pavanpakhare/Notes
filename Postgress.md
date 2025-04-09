# PostgreSQL Database Tutorial

## Table of Contents
1. [Introduction to PostgreSQL](#introduction-to-postgresql)
3. [Basic Commands](#basic-commands)
4. [Creating Databases and Tables](#creating-databases-and-tables)
5. [CRUD Operations](#crud-operations)
6. [Joins and Relationships](#joins-and-relationships)
7. [Indexes and Performance](#indexes-and-performance)
8. [Backup and Restore](#backup-and-restore)
9. [User Management](#user-management)
10. [Advanced Features](#advanced-features)

## Introduction to PostgreSQL

PostgreSQL (often called "Postgres") is a powerful, open-source object-relational database system. It's known for:
- Reliability and data integrity
- Extensive feature set
- Standards compliance
- Excellent performance
- Scalability


## Basic Commands

### Access PostgreSQL
```bash
sudo -u postgres psql
```

### Common psql commands
```sql
\l          -- List all databases
\c dbname   -- Connect to a database
\d          -- List tables in current database
\d tablename -- Describe a table
\q          -- Quit psql
\?          -- Help with psql commands
```

## Creating Databases and Tables

### Create a database
```sql
CREATE DATABASE mydb;
```

### Create a table
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE
);
```

### Create a table with foreign key
```sql
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    title VARCHAR(255) NOT NULL,
    content TEXT,
    published_at TIMESTAMP WITH TIME ZONE
);
```

## CRUD Operations

### Create (INSERT)
```sql
INSERT INTO users (username, email) 
VALUES ('johndoe', 'john@example.com');

INSERT INTO posts (user_id, title, content)
VALUES (1, 'First Post', 'This is my first blog post!');
```

### Read (SELECT)
```sql
-- Select all columns
SELECT * FROM users;

-- Select specific columns
SELECT username, email FROM users;

-- With conditions
SELECT * FROM users WHERE is_active = TRUE;

-- With sorting
SELECT * FROM posts ORDER BY published_at DESC;

-- With limiting
SELECT * FROM posts LIMIT 10;
```

### Update (UPDATE)
```sql
UPDATE users 
SET is_active = FALSE 
WHERE id = 1;

UPDATE posts
SET content = 'Updated content'
WHERE id = 1 AND user_id = 1;
```

### Delete (DELETE)
```sql
DELETE FROM posts WHERE id = 1;

-- Be careful with this one!
DELETE FROM users;  -- Deletes ALL users
```

## Joins and Relationships

### Inner Join
```sql
SELECT users.username, posts.title, posts.published_at
FROM users
INNER JOIN posts ON users.id = posts.user_id;
```

### Left Join
```sql
SELECT users.username, COUNT(posts.id) AS post_count
FROM users
LEFT JOIN posts ON users.id = posts.user_id
GROUP BY users.id;
```

### Many-to-Many Relationship Example
```sql
CREATE TABLE tags (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE post_tags (
    post_id INTEGER REFERENCES posts(id),
    tag_id INTEGER REFERENCES tags(id),
    PRIMARY KEY (post_id, tag_id)
);

-- Query posts with their tags
SELECT posts.title, array_agg(tags.name) AS tags
FROM posts
LEFT JOIN post_tags ON posts.id = post_tags.post_id
LEFT JOIN tags ON post_tags.tag_id = tags.id
GROUP BY posts.id;
```

## Indexes and Performance

### Creating Indexes
```sql
-- Single column index
CREATE INDEX idx_users_email ON users(email);

-- Multi-column index
CREATE INDEX idx_posts_user_published ON posts(user_id, published_at);

-- Unique index (alternative to UNIQUE constraint)
CREATE UNIQUE INDEX idx_unique_username ON users(username);
```

### Analyzing Query Performance
```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'john@example.com';
```

## Backup and Restore

### Backup a database
```bash
pg_dump mydb > mydb_backup.sql
```

### Restore a database
```bash
psql mydb < mydb_backup.sql
```

### Backup all databases
```bash
pg_dumpall > all_databases_backup.sql
```

## User Management

### Create a user
```sql
CREATE USER myuser WITH PASSWORD 'mypassword';
```

### Grant privileges
```sql
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
GRANT SELECT, INSERT ON TABLE posts TO myuser;
```

### Create a read-only user
```sql
CREATE USER readonly WITH PASSWORD 'readonly';
GRANT CONNECT ON DATABASE mydb TO readonly;
GRANT USAGE ON SCHEMA public TO readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly;
```

## Advanced Features

### JSON Support
```sql
-- Create table with JSON column
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    attributes JSONB
);

-- Insert JSON data
INSERT INTO products (name, attributes)
VALUES ('Laptop', '{"color": "silver", "ram": 16, "storage": "512GB SSD"}');

-- Query JSON data
SELECT name, attributes->>'color' AS color
FROM products
WHERE attributes @> '{"ram": 16}';
```

### Full-Text Search
```sql
-- Create a searchable column
ALTER TABLE posts ADD COLUMN search_vector TSVECTOR;

-- Update the search vector
UPDATE posts SET search_vector = 
    to_tsvector('english', title || ' ' || content);

-- Create an index
CREATE INDEX idx_search_vector ON posts USING GIN(search_vector);

-- Perform a search
SELECT title, published_at
FROM posts
WHERE search_vector @@ to_tsquery('english', 'blog & post');
```

### Window Functions
```sql
SELECT 
    username,
    COUNT(posts.id) AS post_count,
    RANK() OVER (ORDER BY COUNT(posts.id) DESC) AS rank
FROM users
LEFT JOIN posts ON users.id = posts.user_id
GROUP BY users.id
LIMIT 10;
```
