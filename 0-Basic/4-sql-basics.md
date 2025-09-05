# SQL Basics in Snowflake

Snowflake uses **ANSI-compliant SQL** as its query language.  
If you already know SQL, most syntax will feel familiar.  
However, Snowflake also introduces some unique extensions and features.

---

## 1. Basic SQL Operations

### SELECT
Retrieve data from a table:
```sql
SELECT first_name, last_name
FROM customers
WHERE country = 'India'
ORDER BY last_name;
```

### INSERT
Add a new row into a table:
```sql
INSERT INTO customers (id, first_name, last_name, country)
VALUES (101, 'Arjun', 'Mehta', 'India');
```

### UPDATE
Modify existing rows:
```sql
UPDATE customers
SET country = 'USA'
WHERE id = 101;
```

### DELETE
Remove rows:
```sql
DELETE FROM customers
WHERE id = 101;
```

---

## 2. Creating Objects

### Create a Database
```sql
CREATE DATABASE sales_db;
```

### Create a Schema
```sql
CREATE SCHEMA analytics;
```

### Create a Table
```sql
CREATE TABLE customers (
    id INT,
    first_name STRING,
    last_name STRING,
    country STRING
);
```

---

## 3. Snowflake-Specific Data Types
- **STRING** → Text  
- **NUMBER / INT** → Numeric values  
- **BOOLEAN** → True/false  
- **DATE / TIMESTAMP** → Date and time values  
- **VARIANT** → Flexible type for semi-structured data (JSON, Avro, Parquet)  

Example with JSON:
```sql
CREATE TABLE orders (
    id INT,
    data VARIANT
);
```

---

## 4. Working with Semi-Structured Data
Snowflake makes querying JSON simple using the `:` operator:
```sql
SELECT 
    data:customer_id AS customer,
    data:items[0].product_name AS first_item
FROM orders;
```

---

## 5. Functions and Expressions
Snowflake provides a rich set of built-in functions:
```sql
-- String function
SELECT UPPER(first_name) FROM customers;

-- Date function
SELECT CURRENT_DATE;

-- Aggregation
SELECT country, COUNT(*) AS total_customers
FROM customers
GROUP BY country;
```

---

## 6. Views
A **view** is a saved query that behaves like a virtual table:
```sql
CREATE VIEW indian_customers AS
SELECT * 
FROM customers
WHERE country = 'India';
```

---

## 7. Key Snowflake Features in SQL
- **Case-insensitive by default** → `CUSTOMERS` and `customers` are treated the same (unless quoted).  
- **Automatic query caching** → repeated queries are faster.  
- **Semi-structured data handling** → direct querying of JSON and nested data.  
- **Time Travel** → query data "as of" a point in the past (covered later).  

---

## 📌 Key Takeaway
Snowflake SQL is **familiar for anyone with SQL knowledge**,  
but its support for **semi-structured data, flexible data types,  
and advanced features** makes it especially powerful in modern data workflows.
