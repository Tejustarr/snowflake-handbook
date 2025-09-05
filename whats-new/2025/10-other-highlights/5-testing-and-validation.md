# Other Highlights — Testing & Validation

How to test correctness, performance, and governance.


## Approaches
- Unit tests (dbt tests or SQL assertions)
- Integration tests (end-to-end pipelines)
- Regression tests (compare metrics before/after)

## Example assertion
```sql
select count(*) from analytics.customers where email is null;
```

---

[Previous](./4-usage-and-scenarios.md) • [Next](./6-ci-cd-and-deployment.md)
