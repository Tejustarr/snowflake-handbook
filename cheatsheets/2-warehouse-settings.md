# Warehouse Settings Cheatsheet

Common warehouse configurations in Snowflake.

---

```sql
-- Create a warehouse
CREATE WAREHOUSE my_wh
WITH WAREHOUSE_SIZE = 'SMALL'
AUTO_SUSPEND = 60
AUTO_RESUME = TRUE;

-- Resize a warehouse
ALTER WAREHOUSE my_wh SET WAREHOUSE_SIZE = 'LARGE';

-- Enable multi-cluster mode
ALTER WAREHOUSE my_wh
SET MIN_CLUSTER_COUNT = 1
    MAX_CLUSTER_COUNT = 5;
```

📌 Use **auto-suspend** + **auto-resume** to save costs.
