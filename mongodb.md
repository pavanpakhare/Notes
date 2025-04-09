# MongoDB Tutorial: The Complete Guide

## Table of Contents
1. [Introduction to MongoDB](#introduction-to-mongodb)
2. [Installation](#installation)
3. [Basic MongoDB Operations](#basic-mongodb-operations)
4. [CRUD Operations](#crud-operations)
5. [Querying Data](#querying-data)
6. [Indexes and Performance](#indexes-and-performance)
7. [Aggregation Framework](#aggregation-framework)
8. [Data Modeling](#data-modeling)
9. [Transactions](#transactions)
10. [Security and User Management](#security-and-user-management)
11. [Backup and Restore](#backup-and-restore)
12. [MongoDB Atlas (Cloud)](#mongodb-atlas-cloud)

## Introduction to MongoDB

MongoDB is a popular NoSQL document database that provides:
- Flexible, JSON-like documents
- Dynamic schemas
- Horizontal scaling
- High performance
- Rich query language

Key concepts:
- **Database**: Container for collections
- **Collection**: Equivalent to a table in RDBMS
- **Document**: Equivalent to a row/record (stored as BSON)
- **Field**: Equivalent to a column

## Installation

### Windows/macOS
Download installer from [mongodb.com/try/download](https://www.mongodb.com/try/download)

### Linux (Ubuntu)
```bash
# Import the public key
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 656408E390CFB1F5

# Create list file
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list

# Update packages
sudo apt update

# Install MongoDB
sudo apt install -y mongodb-org

# Start MongoDB
sudo systemctl start mongod
sudo systemctl enable mongod
```

### Verify Installation
```bash
mongod --version
mongo --version
```

## Basic MongoDB Operations

### Start MongoDB Shell
```bash
mongosh
```

### Common Shell Commands
```javascript
show dbs                // List all databases
use mydb                // Switch to/create database
show collections        // List collections in current db
db.help()              // Show database methods
db.collection.help()   // Show collection methods
exit                   // Exit the shell
```

## CRUD Operations

### Create (Insert)
```javascript
// Insert single document
db.users.insertOne({
  name: "John Doe",
  email: "john@example.com",
  age: 30,
  status: "active",
  joined: new Date()
});

// Insert multiple documents
db.users.insertMany([
  {name: "Alice", email: "alice@example.com", age: 25},
  {name: "Bob", email: "bob@example.com", age: 35}
]);
```

### Read (Find)
```javascript
// Find all documents
db.users.find();

// Find with filter
db.users.find({age: {$gt: 25}});

// Find one document
db.users.findOne({email: "john@example.com"});

// Projection (select fields)
db.users.find({}, {name: 1, email: 1});

// Limit results
db.users.find().limit(5);

// Sort results
db.users.find().sort({age: -1}); // Descending
```

### Update
```javascript
// Update one document
db.users.updateOne(
  {email: "john@example.com"},
  {$set: {status: "inactive"}}
);

// Update multiple documents
db.users.updateMany(
  {age: {$lt: 30}},
  {$inc: {age: 1}} // Increment age by 1
);

// Upsert (insert if not exists)
db.users.updateOne(
  {email: "new@example.com"},
  {$set: {name: "New User", age: 20}},
  {upsert: true}
);
```

### Delete
```javascript
// Delete one document
db.users.deleteOne({email: "john@example.com"});

// Delete multiple documents
db.users.deleteMany({status: "inactive"});

// Delete all documents (be careful!)
db.users.deleteMany({});
```

## Querying Data

### Comparison Operators
```javascript
// Equal
db.users.find({age: 25});

// Greater than
db.users.find({age: {$gt: 25}});

// Less than or equal
db.users.find({age: {$lte: 30}});

// Not equal
db.users.find({status: {$ne: "active"}});

// In array
db.users.find({age: {$in: [25, 30, 35]}});
```

### Logical Operators
```javascript
// AND (implicit)
db.users.find({status: "active", age: {$gt: 25}});

// OR
db.users.find({$or: [{status: "active"}, {age: {$lt: 30}}]});

// NOT
db.users.find({age: {$not: {$gt: 30}}});
```

### Array Operations
```javascript
// Create collection with array
db.products.insertOne({
  name: "Laptop",
  tags: ["electronics", "computers", "portable"],
  price: 999.99
});

// Match array element
db.products.find({tags: "electronics"});

// Match all array elements
db.products.find({tags: {$all: ["electronics", "portable"]}});

// Array size
db.products.find({tags: {$size: 3}});

// Element at position
db.products.find({"tags.0": "electronics"});
```

## Indexes and Performance

### Creating Indexes
```javascript
// Single field index
db.users.createIndex({email: 1}); // 1 for ascending, -1 for descending

// Compound index
db.users.createIndex({status: 1, age: -1});

// Text index for search
db.products.createIndex({name: "text", description: "text"});

// Unique index
db.users.createIndex({email: 1}, {unique: true});
```

### Checking Indexes
```javascript
db.users.getIndexes();
```

### Explain Query
```javascript
db.users.find({email: "john@example.com"}).explain("executionStats");
```

## Aggregation Framework

### Basic Aggregation
```javascript
// Count documents
db.users.countDocuments({status: "active"});

// Group by
db.users.aggregate([
  {$group: {_id: "$status", count: {$sum: 1}}}
]);

// Sort and limit
db.users.aggregate([
  {$sort: {age: -1}},
  {$limit: 5}
]);
```

### Complex Aggregation
```javascript
db.orders.aggregate([
  // Stage 1: Filter documents
  {$match: {status: "completed"}},
  
  // Stage 2: Group by customer and calculate totals
  {$group: {
    _id: "$customer_id",
    totalAmount: {$sum: "$amount"},
    averageAmount: {$avg: "$amount"},
    count: {$sum: 1}
  }},
  
  // Stage 3: Sort by total amount
  {$sort: {totalAmount: -1}},
  
  // Stage 4: Limit to top 10
  {$limit: 10}
]);
```

## Data Modeling

### Embedded Documents
```javascript
// One-to-few: Embed directly
db.users.insertOne({
  name: "John",
  address: {
    street: "123 Main St",
    city: "New York",
    zip: "10001"
  }
});
```

### Document References
```javascript
// One-to-many: Use references
db.authors.insertOne({
  _id: 1,
  name: "J.K. Rowling"
});

db.books.insertMany([
  {title: "Harry Potter 1", author_id: 1},
  {title: "Harry Potter 2", author_id: 1}
]);
```

### Many-to-Many
```javascript
// Using array of references
db.students.insertOne({
  _id: 1,
  name: "Alice",
  courses: [101, 102]
});

db.courses.insertMany([
  {_id: 101, name: "Math"},
  {_id: 102, name: "History"}
]);
```

## Transactions

### Basic Transaction
```javascript
const session = db.getMongo().startSession();
session.startTransaction();

try {
  const users = session.getDatabase("mydb").users;
  const accounts = session.getDatabase("mydb").accounts;
  
  users.deleteOne({_id: 123});
  accounts.deleteMany({user_id: 123});
  
  session.commitTransaction();
} catch (error) {
  session.abortTransaction();
  throw error;
} finally {
  session.endSession();
}
```

## Security and User Management

### Creating Users
```javascript
use admin
db.createUser({
  user: "admin",
  pwd: "securepassword",
  roles: ["root"]
});

use mydb
db.createUser({
  user: "appuser",
  pwd: "apppassword",
  roles: ["readWrite"]
});
```

### Authentication
```bash
mongosh -u admin -p securepassword --authenticationDatabase admin
```

### Role Management
```javascript
// Grant additional roles
db.grantRolesToUser("appuser", ["readWrite", "dbAdmin"]);

// Revoke roles
db.revokeRolesFromUser("appuser", ["dbAdmin"]);
```

## Backup and Restore

### Export (mongodump)
```bash
mongodump --db mydb --out /backup/
```

### Import (mongorestore)
```bash
mongorestore /backup/mydb/
```

### Export Collection (mongoexport)
```bash
mongoexport --db mydb --collection users --out users.json
```

### Import Collection (mongoimport)
```bash
mongoimport --db mydb --collection users --file users.json
```

## MongoDB Atlas (Cloud)

MongoDB Atlas is the fully-managed cloud database service:

1. Go to [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Create free account (free tier available)
3. Create a cluster
4. Whitelist your IP address
5. Get connection string
6. Connect using:

```javascript
mongosh "mongodb+srv://username:password@cluster.mongodb.net/mydb"
```

### Atlas Features
- Automated backups
- Scaling with click
- Built-in performance advisor
- Global clusters
- Serverless options

