# Core Concepts in Snowflake

Before diving deeper into Snowflake, it’s important to understand the **fundamental building blocks** that make up the platform. These concepts form the foundation for everything else.

---

## 1. Databases, Schemas, and Tables
- **Database** → A logical container for schemas and data.  
- **Schema** → A logical grouping of objects (tables, views, stages, etc.) inside a database.  
- **Table** → Stores structured data in rows and columns.  

📌 Example structure:  
```
Database: SALES_DB
   └── Schema: ANALYTICS
        ├── Table: CUSTOMERS
        ├── Table: ORDERS
        └── View: CUSTOMER_SUMMARY
```

---

## 2. Virtual Warehouses
- Independent **compute clusters** used to run queries, load data, or perform transformations.  
- Each warehouse can:
  - Scale **up** (larger size → faster performance)  
  - Scale **out** (multi-cluster → handle concurrency)  
- Warehouses can be **paused** to save costs and **resumed on demand**.  

---

## 3. Stages
- **Stages** are storage locations used to **load/unload data** in Snowflake.  
- Types of stages:
  - **Internal stage** → Managed inside Snowflake.  
  - **External stage** → Points to cloud storage (e.g., AWS S3, Azure Blob, GCP GCS).  

📌 Example: Upload files to a stage, then use `COPY INTO` to load them into a table.  

---

## 4. File Formats
- Snowflake supports multiple formats for data loading/unloading:  
  - CSV  
  - JSON  
  - Avro  
  - ORC  
  - Parquet  
- File formats are defined as reusable objects for consistency.  

---

## 5. Roles and Users
- Snowflake uses **Role-Based Access Control (RBAC)**.  
- Key roles:
  - **ACCOUNTADMIN** → Full control  
  - **SYSADMIN** → Manage objects (databases, schemas)  
  - **USERADMIN** → Manage users & roles  
  - **PUBLIC** → Default minimal-access role  

Users are assigned **roles**, and roles define permissions.  

---

## 6. Query Processing
- Queries are written in **Snowflake SQL** (a dialect of ANSI SQL).  
- The **Cloud Services Layer** handles parsing, optimization, and metadata.  
- The **Compute Layer** (warehouse) executes the query.  
- Results are returned almost instantly, and cached for faster repeats.  

---

## 7. Semi-Structured Data
- Snowflake natively supports **JSON, Avro, ORC, Parquet** using the `VARIANT` data type.  
- You can store semi-structured data in a column and query it using SQL.  

Example:
```sql
SELECT 
    data:customer_id AS customer_id,
    data:order_total AS total
FROM raw_orders;
```

---

## 📌 Key Takeaway
The **core concepts** of Snowflake — databases, schemas, tables, warehouses, stages, file formats, and roles — give you the **foundation to manage and query data**. Once you’re familiar with these, you’ll be ready to start exploring how to work with data in Snowflake effectively.
