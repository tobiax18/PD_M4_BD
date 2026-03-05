# 📦 MegaStore Global – Hybrid Data Architecture

## 📌 Project Overview

This project is a migration and modernization solution for **MegaStore Global**, a retail company that previously managed all its operations in a single Excel file.

The objective of this project is to:

- Normalize and structure legacy flat data
- Design a hybrid persistence architecture
- Implement both SQL and NoSQL databases
- Develop a RESTful API using Node.js and Express
- Provide Business Intelligence queries
- Implement audit logging using MongoDB

---

# 🏗 Architecture Overview

The system uses a **Hybrid Database Architecture**:

| Database | Purpose |
|----------|----------|
| PostgreSQL | Structured transactional data |
| MongoDB | Audit logging (deleted products) |

### Why Hybrid Architecture?

### PostgreSQL is used because:
- ACID compliance
- Referential integrity with Foreign Keys
- 3NF normalization
- Complex JOIN queries for analytics
- Structured transactional operations

### MongoDB is used because:
- Flexible document storage
- High write performance
- Ideal for audit logs
- No need for relational constraints

---

# 🐘 Relational Database Design (PostgreSQL)

Database Name:

```
db_megastore_exam
```

## Tables Structure

### 1. customers
- id (PK)
- name
- email (UNIQUE)
- address

### 2. suppliers
- id (PK)
- name (UNIQUE)
- contact

### 3. categories
- id (PK)
- name (UNIQUE)

### 4. products
- id (PK)
- sku (UNIQUE)
- name
- unit_price
- category_id (FK)
- supplier_id (FK)

### 5. orders
- id (PK)
- transaction_id (UNIQUE)
- order_date
- customer_id (FK)

### 6. order_details
- id (PK)
- order_id (FK)
- product_id (FK)
- quantity

---

## ✅ Normalization Process (3NF)

### First Normal Form (1NF)
- Removed repeating groups.
- Ensured atomic values.

### Second Normal Form (2NF)
- Eliminated partial dependencies.
- Separated orders from products.

### Third Normal Form (3NF)
- Eliminated transitive dependencies.
- Separated suppliers, categories, and customers into independent tables.

This guarantees:
- No data redundancy
- Data integrity
- Scalability
- Clean relational structure

---

# 🍃 NoSQL Database Design (MongoDB)

Database Name:

```
db_megastore_exam
```

Collection:

```
deleted_products_log
```

## Document Structure

```json
{
  "productId": 10,
  "deletedAt": "2026-03-05T10:00:00Z"
}
```

## Schema Validation

- productId → Number (required)
- deletedAt → Date (required)

### Why not store transactional data in MongoDB?

- Orders require JOIN operations.
- Strong referential integrity required.
- SQL is better suited for transactional systems.

MongoDB is exclusively used for audit logging to maintain a clean hybrid architecture.

---

# ⚙️ Installation & Setup Guide

## 1️⃣ Install PostgreSQL

Download:
https://www.postgresql.org/download/

Create database:

```sql
CREATE DATABASE db_megastore_exam;
```

Connect:

```sql
\c db_megastore_exam
```

Run the DDL scripts located in `/docs`.

---

## 2️⃣ Install MongoDB

Download:
https://www.mongodb.com/try/download/community

Start Mongo shell:

```
mongosh
```

Create database:

```
use db_megastore_exam
```

Run schema validation script from `/docs`.

---

## 3️⃣ Clone Repository

```
git clone <your-repository-url>
cd megastore-api
```

---

## 4️⃣ Install Dependencies

```
npm install
```

---

## 5️⃣ Configure Environment Variables

Create a `.env` file in root directory:

```
PORT=3000

PG_USER=postgres
PG_HOST=localhost
PG_DATABASE=db_megastore_exam
PG_PASSWORD=yourpassword
PG_PORT=5432

MONGO_URI=mongodb://localhost:27017/db_megastore_exam
```

---

## 6️⃣ Start Server

```
node server.js
```

Server will run at:

```
http://localhost:3000
```

---

# 📥 Bulk Data Migration

The system includes a migration script that reads a raw CSV file and distributes the data into normalized tables.

Run migration:

```
node migration/migrate.js
```

## Idempotency Logic

Before inserting master entities, the system checks:

- If customer email already exists
- If supplier already exists
- If category already exists
- If product SKU already exists
- If transaction_id already exists

If record exists → reuse ID  
If not → insert new record  

This prevents:
- Duplicate customers
- Duplicate suppliers
- Duplicate products
- Duplicate transactions

---

# 🔁 REST API Endpoints

Base URL:

```
http://localhost:3000
```

---

## 📦 Products CRUD

### Create Product
```
POST /products
```

### Get All Products
```
GET /products
```

### Get Product by ID
```
GET /products/:id
```

### Update Product
```
PUT /products/:id
```

### Delete Product
```
DELETE /products/:id
```

When deleting:
- Product is removed from PostgreSQL
- A deletion log is stored in MongoDB

---

# 📊 Business Intelligence Queries

---

## 1️⃣ Supplier Analysis

Which suppliers sold the most items and what is their total inventory value?

Uses:
- JOIN
- GROUP BY
- SUM
- ORDER BY

---

## 2️⃣ Customer Purchase History

Displays:
- Products purchased
- Purchase dates
- Total spent per transaction

---

## 3️⃣ Top Selling Products by Category

Displays:
- Products inside a category
- Total quantity sold
- Total revenue generated
- Ordered by revenue

---

# 🧪 Testing with Postman

Import the Postman collection located in `/docs`.

Test:

- CRUD endpoints
- Business intelligence endpoints
- Deletion logging

---

# 📁 Project Structure

```
megastore-api
│
├── config
│   ├── postgres.js
│   └── mongo.js
│
├── controllers
├── routes
├── migration
│
├── docs
│   ├── DDL.sql
│   ├── mongo_validation.js
│   ├── sample_data.csv
│   └── diagrams.pdf
│
├── server.js
├── .env
└── README.md
```

---

# 🚀 Performance Improvements

- Indexes on foreign keys
- Unique constraints
- Schema validation in MongoDB
- Environment variables for security
- Modular backend architecture

---

# 🎯 Acceptance Criteria Coverage

✔ 3NF applied  
✔ Referential integrity enforced  
✔ MongoDB schema validation  
✔ Idempotent migration  
✔ RESTful API  
✔ Audit logging  
✔ Business intelligence queries  

---

# 🏁 Conclusion

This project demonstrates:

- Proper relational modeling
- Hybrid database architecture
- Backend modular design
- Safe migration strategy
- Audit log implementation
- Analytical query capabilities

The solution is scalable, maintainable, and aligned with enterprise backend best practices.
