# Machine Learning Enhancements — Setup

Setup steps, enabling features, and security considerations.


## Enable ML Features & Create Model Role
```sql
alter account set ENABLE_ML = true;
create role ml_trainer;
grant usage on warehouse ml_wh to role ml_trainer;
```
## Example: register model storage location
```sql
create or replace file format ml_model_format type = 'PARQUET';
```

---

[Previous](./2-intro.md) • [Next](./4-usage-and-scenarios.md)
