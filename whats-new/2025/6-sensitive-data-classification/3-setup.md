# Sensitive Data Classification — Setup

Setup steps, enabling features, and security considerations.


## Enable Classification & Masking
Use Snowflake's classification functions and labeling. Example command to run classification jobs:

```sql
call system$classify_table('ANALYTICS', 'CUSTOMERS');
```

## Create masking policy
```sql
create or replace masking policy mask_email as (val string) returns string ->
  case when current_role() in ('ANALYST_ROLE') then val else '***REDACTED***' end;
```

---

[Previous](./2-intro.md) • [Next](./4-usage-and-scenarios.md)
