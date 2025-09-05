# Performance Tuning in Snowflake

Snowflake is designed to be performant out of the box, but as data grows, optimizing queries and warehouses becomes critical.  
Performance tuning in Snowflake focuses on **warehouse sizing, caching, partitioning, clustering, and query design**.

---

## 1. Query Caching

Snowflake automatically caches query results.

- **Result Cache** → Stores the results of queries for 24 hours.  
- **Metadata Cache** → Stores table metadata (micro-partitions, statistics).  
- **Warehouse Cache** → Stores data in local SSDs of the warehouse while it is running.  

📌 Benefit: If you run the same query twice without changes in underlying data, the second run is nearly instantaneous.

---

## 2. Micro-Partitions

- Data in Snowflake is automatically divided into **micro-partitions** (~16 MB each).  
- Each partition stores metadata (min/max values, counts).  
- Queries only scan the partitions relevant to the query → **pruning**.  

Example:
```sql
SELECT * FROM orders WHERE order_date = '2025-09-01';
```
👉 Only partitions that contain this date will be scanned.

---

## 3. Clustering

Clustering helps optimize query performance when filtering on specific columns.

### Automatic Clustering
- Snowflake organizes micro-partitions automatically.  
- Works well for most use cases.

### Manual Clustering
- You can define a **cluster key** for large tables where queries often filter on the same column(s).  
- Snowflake will reorganize data for faster pruning.

Example:
```sql
ALTER TABLE orders
CLUSTER BY (customer_id);
```

---

## 4. Warehouse Sizing and Scaling

- **Start small** (XS or S) → scale up only if queries are slow.  
- **Scale up (vertical scaling)** → faster query execution for large jobs.  
- **Scale out (horizontal scaling)** → add clusters for high concurrency.  
- Use **auto-suspend** to save costs and **auto-resume** for flexibility.

---

## 5. Query Design Best Practices

- **SELECT only needed columns** → avoid `SELECT *`.  
- **Filter early** → reduce data scanned.  
- **Use proper data types** → avoid unnecessary casting.  
- **Materialize frequent joins or aggregations** → use views or materialized views.  
- **Avoid excessive DISTINCT** unless necessary.  
- **Leverage CTEs (WITH clauses)** for clarity, but check for repeated execution overhead.

---

## 6. Materialized Views

- Precompute and store query results for faster access.  
- Automatically updated as underlying data changes.  
- Great for dashboards or frequently queried aggregates.

Example:
```sql
CREATE MATERIALIZED VIEW customer_sales AS
SELECT customer_id, SUM(amount) AS total_sales
FROM orders
GROUP BY customer_id;
```

---

## 7. Monitoring Query Performance

- Use the **Query Profile** tool in Snowsight:
  - Visualizes execution steps.  
  - Shows scanned partitions, join operations, and bottlenecks.  
- Helps identify slow queries and optimize them.

---

## 📌 Key Takeaway
Performance tuning in Snowflake is less about manual tweaking and more about **smart design choices**:  
- Use caching effectively.  
- Let micro-partition pruning do the heavy lifting.  
- Apply clustering when queries demand it.  
- Right-size warehouses and design efficient SQL.  
Together, these ensure **fast queries at optimized cost**.
