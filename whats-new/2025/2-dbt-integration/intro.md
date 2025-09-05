# Introduction to Snowflake + dbt Integration

dbt (data build tool) and Snowflake together form a widely adopted pattern: **ingest raw data, model inside the warehouse, and serve curated datasets**. This approach is commonly called ELT (Extract, Load, Transform). Why is this compelling? Because modern warehouses like Snowflake are highly scalable, and dbt provides structure, testing, and software-engineering principles for SQL workflows.

## Why this combo matters — short list
- **Simplicity**: use SQL & YAML instead of custom codebase + glue jobs.
- **Performance**: Snowflake executes queries in the cloud; dbt compiles optimized SQL.
- **Governance**: models live in version control, with tests and documented schemas.
- **Cost control**: incremental models + dedicated warehouses reduce compute cost.
- **Team productivity**: analytics engineers ship reliable logic with review processes.

## A story: from raw log to dashboard
Imagine a product team that wants a daily active users (DAU) metric. Raw clickstream lands in a `raw.events` table in Snowflake via ingestion tools. dbt:

1. Creates `stg_events` with normalization & basic cleaning.
2. Creates `fct_events` as an incremental model that deduplicates events and updates counts using `MERGE`.
3. Runs tests to check nullability and uniqueness.
4. Deploys to Production via CI/CD when PRs are merged.
5. BI queries `analytics.dau` for dashboards.

This flow reduces operational friction and keeps transformation logic auditable.

## Who benefits?
- Analytics engineers: reproducible pipelines and test-driven development.
- Data platform teams: standardized deployment and cost visibility.
- Data consumers (analysts & product): trustable, documented datasets.

## Quick architecture diagram

```mermaid
graph TD
  A[Ingest: Connectors (Fivetran/Airbyte)] --> B[Snowflake Raw Layer]
  B --> C[dbt Staging Models]
  C --> D[dbt Marts/Models (Incremental)]
  D --> E[BI Tools / Consumers]
  CI[CI/CD Pipeline] --> C
  CI --> D
  Monitoring --> D
```

---

Next: [setup.md](./setup.md) — practical installation and secure config.