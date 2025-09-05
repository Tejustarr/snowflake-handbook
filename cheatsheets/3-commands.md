# Useful Snowflake Commands Cheatsheet

---

## Session Info
```sql
SELECT CURRENT_USER(), CURRENT_ROLE(), CURRENT_WAREHOUSE();
```

## Switching Context
```sql
USE DATABASE sales_db;
USE SCHEMA analytics;
USE ROLE analyst;
USE WAREHOUSE my_wh;
```

## Monitoring
```sql
SHOW WAREHOUSES;
SHOW DATABASES;
SHOW USERS;
```

## Security
```sql
CREATE ROLE analyst;
GRANT SELECT ON TABLE customers TO ROLE analyst;
GRANT ROLE analyst TO USER arjun;
```
