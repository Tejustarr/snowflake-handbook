# SQL Cheatsheet for Snowflake

Quick reference for common SQL commands.

---

## Database and Schema
```sql
CREATE DATABASE mydb;
CREATE SCHEMA mydb.analytics;
```

## Table Management
```sql
CREATE TABLE customers (
    id INT, name STRING, country STRING
);
DROP TABLE customers;
```

## Basic Queries
```sql
SELECT * FROM customers;
SELECT id, name FROM customers WHERE country = 'India';
```

## Inserts and Updates
```sql
INSERT INTO customers VALUES (1, 'Arjun', 'India');
UPDATE customers SET country = 'USA' WHERE id = 1;
```

## Views
```sql
CREATE VIEW indian_customers AS
SELECT * FROM customers WHERE country = 'India';
```
