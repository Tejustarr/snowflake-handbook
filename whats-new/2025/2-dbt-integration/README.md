# Snowflake + dbt Integration (Feb 2025)

Welcome to the **Snowflake + dbt Integration** study collection — a deep, practical, and opinionated learning pack for analytics engineers, data platform engineers, and curious data professionals. This folder contains guided lessons, hands-on patterns, code snippets, CI/CD examples, performance guidance, and curated resources so you can run production-grade dbt on Snowflake with confidence.

---

## 📚 What’s inside this collection

- [intro.md](./intro.md) — Why dbt and Snowflake form a modern ELT powerhouse. Big-picture rationale and adoption patterns.
- [setup.md](./setup.md) — Step-by-step configuration for dbt Cloud and dbt Core, secure profiles, key-pair auth and GitHub Actions.
- [models-and-transformations.md](./models-and-transformations.md) — Model design, materializations, incremental patterns, snapshots, macros, and Snowflake-specific SQL recipes.
- [testing-and-documentation.md](./testing-and-documentation.md) — dbt tests, custom tests, docs site, auto-generated lineage, and governance patterns.
- [ci-cd.md](./ci-cd.md) — Continuous integration and deployment strategies, GitHub Actions and deployment promotion flows.
- [performance-and-best-practices.md](./performance-and-best-practices.md) — Query optimization, warehouse sizing, clustering, costs and monitoring.
- [resources.md](./resources.md) — Curated links, example repos, blogs, and community channels.

---

## 🎯 Learning outcomes (what you’ll be able to do)

- Architect a dbt project designed to run efficiently on Snowflake.
- Implement safe incremental pipelines and snapshot patterns using MERGE and key-pair auth.
- Build a CI/CD pipeline that runs tests, docs, and promotes models across environments.
- Apply Snowflake-specific performance and cost optimizations for dbt workloads.
- Create reproducible documentation and lineage to satisfy stakeholders and auditors.

---

## 🚀 Quick graph (how things connect)

```mermaid
flowchart LR
  RawData[Raw sources (S3, Streams, Connectors)]
  Ingest[Ingest into Snowflake]
  Staging[dbt: staging models]
  Marts[dbt: marts & aggregates]
  Tests[dbt tests & docs]
  BI[BI / Analytics]
  CI[CI/CD]
  Monitoring[Monitoring & Cost Management]

  RawData --> Ingest --> Staging --> Marts --> BI
  Staging --> Tests --> Marts
  CI --> Tests
  Marts --> Monitoring
```

---

Ready? Start with [intro.md](./intro.md).