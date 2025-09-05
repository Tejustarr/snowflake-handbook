# Cost Optimization in Snowflake

Snowflake follows a **pay-as-you-go model**, which provides flexibility but also requires smart cost management.  
Costs mainly come from **compute (warehouses)**, **storage**, and **data transfer**.  
By applying best practices, you can keep costs predictable and efficient.

---

## 1. Understanding Snowflake Pricing

### Compute Costs
- Charged per-second while a **warehouse** is running.  
- Larger warehouses cost more but process faster.  
- Idle warehouses can be suspended to stop charges.  

### Storage Costs
- Charged per TB per month.  
- Includes tables, stages, clones, and Time Travel data.  

### Data Transfer Costs
- Charged for **cross-region** or **cross-cloud** data sharing/replication.  
- Queries within the same region usually do not incur transfer costs.  

---

## 2. Warehouse Optimization

- Use **auto-suspend** (e.g., 60 seconds) to stop warehouses when idle.  
- Use **auto-resume** so they start automatically when needed.  
- Start with **smaller warehouses (XS/S)** and scale up only if necessary.  
- Separate workloads (ETL, BI, Ad-hoc) into different warehouses for better cost control.  

Example:
```sql
ALTER WAREHOUSE my_wh
SET AUTO_SUSPEND = 60
AUTO_RESUME = TRUE
WAREHOUSE_SIZE = 'SMALL';
```

---

## 3. Query Optimization

- Avoid `SELECT *`; fetch only the required columns.  
- Filter early using `WHERE` clauses to reduce scanned data.  
- Use clustering for large tables to reduce scan size.  
- Leverage **result caching** for repeated queries.  
- Monitor queries with the **Query Profile** tool to identify bottlenecks.  

---

## 4. Storage Optimization

- Drop unused tables, clones, and temporary objects.  
- Set appropriate **Time Travel retention** (shorter = cheaper).  
- Compress and partition external files (Parquet, ORC).  
- Use **streams and tasks cleanup** to avoid metadata bloat.  

---

## 5. Resource Monitors

Snowflake provides **resource monitors** to track and control compute spend.

Example:
```sql
-- Create a resource monitor with a monthly quota
CREATE RESOURCE MONITOR monthly_quota
WITH CREDIT_QUOTA = 500
FREQUENCY = MONTHLY
START_TIMESTAMP = '2025-09-01 00:00:00';

-- Assign monitor to a warehouse
ALTER WAREHOUSE my_wh
SET RESOURCE_MONITOR = monthly_quota;
```

- Can trigger notifications or suspend warehouses when limits are reached.  
- Helps prevent runaway costs from unexpected workloads.  

---

## 6. Data Sharing and Replication Costs

- Use **secure data sharing** instead of duplicating datasets.  
- Limit **cross-region replication** to compliance-critical data.  
- Monitor **egress fees** when sharing data across clouds/regions.  

---

## 7. Best Practices Checklist

- ✅ Enable **auto-suspend/auto-resume** on all warehouses.  
- ✅ Start with the **smallest warehouse size** possible.  
- ✅ Use **resource monitors** to cap spending.  
- ✅ Optimize queries and leverage caching.  
- ✅ Review **storage usage** (drop old clones, shorten retention).  
- ✅ Be strategic with **cross-region data transfers**.  

---

## 📌 Key Takeaway
Snowflake’s flexibility can lead to cost savings — **if managed properly**.  
Focus on **warehouse right-sizing, query optimization, storage management, and resource monitors** to get maximum value while keeping bills under control.
