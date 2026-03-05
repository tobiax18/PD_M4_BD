📦 MegaStore Global – Hybrid Data Architecture Exam
📌 Project Overview

This project is a migration and modernization solution for MegaStore Global, a retail company that previously managed all its operations in a single Excel file.

The objective is to:

Normalize and structure legacy flat data

Design a hybrid persistence architecture

Implement both SQL and NoSQL databases

Develop a RESTful API using Node.js

Provide Business Intelligence queries

Implement audit logging

🏗 Architecture Overview

The system uses a Hybrid Database Architecture:

Database	Purpose
PostgreSQL	Structured transactional data
MongoDB	Audit logging (deleted products)
Why Hybrid?

PostgreSQL ensures:

ACID compliance

Referential integrity

3NF normalization

Complex JOIN queries

MongoDB is used for:

Flexible log storage

High write performance

Non-relational audit records

🐘 Relational Database Design (PostgreSQL)

Database Name:

db_megastore_exam
Tables
1. customers

id (PK)

name

email (UNIQUE)

address

2. suppliers

id (PK)

name (UNIQUE)

contact

3. categories

id (PK)

name (UNIQUE)

4. products

id (PK)

sku (UNIQUE)

name

unit_price

category_id (FK)

supplier_id (FK)

5. orders

id (PK)

transaction_id (UNIQUE)

order_date

customer_id (FK)

6. order_details

id (PK)

order_id (FK)

product_id (FK)

quantity

✅ Normalization Process (3NF)
1NF

Removed repeating groups.

Ensured atomic values.

2NF

Removed partial dependencies.

Separated products from orders.

3NF

Removed transitive dependencies.

Suppliers and categories separated from products.

Customers separated from orders.

This eliminates redundancy and guarantees data consistency.

🍃 NoSQL Database Design (MongoDB)

Database Name:

db_megastore_exam

Collection:

deleted_products_log
Schema Validation

Fields:

productId (Number)

deletedAt (Date)

Why Not Store Orders in Mongo?

Orders require JOIN operations.

Strong relational integrity required.

SQL is more appropriate for transactional systems.

MongoDB is used exclusively for audit logging, ensuring:

Flexible structure

Independent scaling

High-performance insert operations

⚙️ Installation & Setup Guide
1️⃣ Install PostgreSQL

Download from:
https://www.postgresql.org/download/

Create database:

CREATE DATABASE db_megastore_exam;

Connect:

\c db_megastore_exam

Run the SQL DDL scripts inside /docs.

2️⃣ Install MongoDB

Download:
https://www.mongodb.com/try/download/community

Start Mongo shell:

mongosh

Create database:

use db_megastore_exam

Run validation script inside /docs.

3️⃣ Clone the Repository
git clone <your-repo-url>
cd megastore-api
4️⃣ Install Dependencies
npm install
5️⃣ Configure Environment Variables

Create a .env file:

PORT=3000

PG_USER=postgres
PG_HOST=localhost
PG_DATABASE=db_megastore_exam
PG_PASSWORD=yourpassword
PG_PORT=5432

MONGO_URI=mongodb://localhost:27017/db_megastore_exam
6️⃣ Start Server
node server.js

Server runs on:

http://localhost:3000
📥 Bulk Migration Guide

The system includes a migration script that reads a raw CSV file and distributes the data into normalized tables.

Run migration:

node migration/migrate.js
Idempotency Logic

Before inserting master entities:

Check if customer email already exists

Check if supplier already exists

Check if category already exists

Check if product SKU already exists

Check if transaction_id already exists

If entity exists → reuse its ID
If not → create new record

This guarantees:

No duplicated customers

No duplicated suppliers

No duplicated products

No duplicated orders

🔁 REST API Endpoints

Base URL:

http://localhost:3000
📦 Products CRUD
Create Product
POST /products
Get All Products
GET /products
Get Product by ID
GET /products/:id
Update Product
PUT /products/:id
Delete Product
DELETE /products/:id

When deleting:

Product is removed from PostgreSQL

A log entry is created in MongoDB

📊 Business Intelligence Queries

These queries can be tested in Postman.

1️⃣ Supplier Analysis

“Which suppliers sold the most items and what is their inventory value?”

Uses:

JOIN

GROUP BY

SUM

ORDER BY

2️⃣ Customer Purchase History

“Show full purchase history of a specific customer.”

Includes:

Products

Dates

Total spent per transaction

3️⃣ Top Selling Products by Category

“List best-selling products inside a specific category ordered by revenue.”

Uses:

Aggregations

Revenue calculation

🧪 Testing with Postman

Import the provided Postman collection inside /docs.

Test:

CRUD endpoints

Business Intelligence endpoints

Deletion logging

📁 Project Structure
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
🚀 Performance Improvements (Bonus)

Index on foreign keys

Unique constraints

Schema validation in MongoDB

Modular architecture

Environment variables for security

🎯 Acceptance Criteria Coverage

✔ 3NF applied
✔ Referential integrity enforced
✔ Mongo schema validation
✔ Idempotent migration
✔ RESTful API
✔ Audit logging
✔ Business intelligence queries

🏁 Conclusion

This project demonstrates:

Proper relational modeling

Hybrid database architecture design

Clean backend structure

Data migration strategy

Audit log implementation

Business intelligence capability

The system is scalable, maintainable, and aligned with enterprise backend best practices.
