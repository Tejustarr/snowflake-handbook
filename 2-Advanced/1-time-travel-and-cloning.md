# Time Travel and Cloning in Snowflake

One of Snowflake’s unique advantages is the ability to **query data in the past** and to create **clones of objects instantly** without duplicating data.  
These features simplify recovery, testing, and development while saving time and storage.

---

## 1. Time Travel

### What is Time Travel?
- Snowflake automatically preserves historical versions of data for a defined period.  
- You can **query, restore, or clone** data “as of” a past timestamp or statement.  

### Default Retention Period
- Standard accounts: **1 day (24 hours)**.  
- Enterprise editions: Up to **90 days** (configurable).  

---

### Using Time Travel

**Querying past data**
```sql
-- Query a table as of yesterday
SELECT * FROM orders
AT (OFFSET => -60*60*24); -- 1 day ago in seconds

-- Query a table as of a specific timestamp
SELECT * FROM orders
AT (TIMESTAMP => '2025-09-01 12:00:00');
```

**Restoring a dropped object**
```sql
-- Restore a dropped table
UNDROP TABLE orders;

-- Restore a schema
UNDROP SCHEMA analytics;
```

**Creating a clone from past data**
```sql
CREATE TABLE orders_yesterday CLONE orders
AT (TIMESTAMP => '2025-09-01 12:00:00');
```

---

## 2. Cloning

### What is Cloning?
- Creates a **zero-copy replica** of an object (table, schema, database).  
- The clone references the same underlying data but appears as a full copy.  
- Since no data is duplicated, clones are:
  - **Instant to create**
  - **Space-efficient**

### Supported Objects
- Tables  
- Schemas  
- Databases  

---

### Using Cloning

**Clone a table**
```sql
CREATE TABLE orders_clone CLONE orders;
```

**Clone a schema**
```sql
CREATE SCHEMA analytics_dev CLONE analytics;
```

**Clone a database**
```sql
CREATE DATABASE sales_dev CLONE sales;
```

---

## 3. How Time Travel and Cloning Work Together

- **Cloning + Time Travel** → You can create a clone of an object as of a point in the past.  
- Example:
```sql
CREATE TABLE orders_backup CLONE orders
AT (OFFSET => -3600); -- Clone table as it was 1 hour ago
```

- This allows:
  - Safe experiments on past versions of data.  
  - Quick recovery from accidental deletes or updates.  
  - Creating dev/test environments instantly.  

---

## 4. Best Practices

- Use **cloning** for dev/test environments instead of duplicating tables.  
- Regularly use **Time Travel** for backup and recovery validation.  
- Be mindful of the **retention period** (extend if needed for compliance).  
- Drop unnecessary clones to reduce metadata clutter.  

---

## 📌 Key Takeaway
**Time Travel** allows you to **query and restore historical data**,  
while **Cloning** lets you create **instant, zero-copy copies** of objects.  
Together, they give Snowflake powerful **data recovery and development capabilities** that save both time and cost.
