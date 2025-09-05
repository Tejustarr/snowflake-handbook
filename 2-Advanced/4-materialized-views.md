# Materialized Views in Snowflake

A **materialized view** is a database object that stores the results of a query physically, rather than recomputing them every time.  
In Snowflake, materialized views are especially useful for **frequently used aggregations, filters, and joins**.

---

## 1. What is a Materialized View?

- A **precomputed, stored result set** of a query.  
- Unlike regular views (which are virtual), materialized views save data on disk.  
- Snowflake automatically keeps them up to date as the underlying data changes.  

📌 Benefit: Queries on materialized views are **much faster** than running the same query on raw tables.

---

## 2. Creating a Materialized View

Example:  
```sql
CREATE MATERIALIZED VIEW customer_sales_mv AS
SELECT customer_id, SUM(amount) AS total_sales
FROM orders
GROUP BY customer_id;
```

Now, whenever you query `customer_sales_mv`, you get instant results.

---

## 3. Refreshing Materialized Views

- Snowflake **automatically refreshes** materialized views when the base table changes.  
- Refresh is incremental (only processes new/changed partitions).  
- Users don’t need to manually trigger updates.  

---

## 4. Limitations

- Only **one base table** is allowed in the FROM clause.  
- Cannot include:
  - Non-deterministic functions (`CURRENT_DATE`, `RANDOM()` etc.)  
  - Joins across multiple base tables (except lookups with constants).  
  - Nested views or subqueries with unsupported operations.  
- Consumes **additional storage** since data is persisted.  

---

## 5. Using a Materialized View

Queries automatically use the materialized view if:
- It covers the same columns and filters.  
- The query optimizer decides it’s more efficient.  

Example:
```sql
-- BI dashboard query
SELECT customer_id, total_sales
FROM customer_sales_mv
WHERE total_sales > 1000;
```

---

## 6. Monitoring and Managing

- Use **ACCOUNT_USAGE.MATERIALIZED_VIEWS** to monitor usage.  
- Check **storage consumption** since materialized views occupy space.  
- Drop unused views to save cost:
```sql
DROP MATERIALIZED VIEW customer_sales_mv;
```

---

## 7. Best Practices

- Create materialized views for:
  - Frequently queried aggregates (e.g., sales totals, counts).  
  - BI dashboards that refresh often.  
- Avoid using them for:
  - Rarely queried data (not worth storage cost).  
  - Highly volatile tables (refresh overhead may outweigh benefits).  
- Monitor cost vs. performance benefit regularly.  

---

## 📌 Key Takeaway
Materialized views in Snowflake **trade storage for speed**.  
They are perfect for **frequent, repetitive queries** — especially in BI dashboards —  
but should be used selectively to balance **performance gains and storage costs**.
