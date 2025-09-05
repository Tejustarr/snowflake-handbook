# Dynamic Tables — Setup

Setup steps, enabling features, and security considerations.


## Enable Dynamic Tables & create a dynamic table
```sql
show parameters like 'ENABLE_DYNAMIC_TABLES';
alter account set ENABLE_DYNAMIC_TABLES = true;

create or replace dynamic table daily_agg target_lag='1 minute' warehouse=etl_wh as
  select user_id, count(*) as events from raw.events group by user_id;
```

---

[Previous](./2-intro.md) • [Next](./4-usage-and-scenarios.md)
