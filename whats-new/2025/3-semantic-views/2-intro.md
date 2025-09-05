# Introduction to Snowflake Semantic Views

## ❓ What are Semantic Views?
Semantic Views are Snowflake-managed objects that define **metrics, dimensions, and relationships** in a consistent, reusable layer.  
They bridge the gap between raw tables and downstream tools by serving as the **single source of truth** for business logic.

---

## 🌟 Why Semantic Views?

### Traditional Problem
- BI teams define KPIs (Revenue, DAU, Retention) differently across dashboards.
- Analysts rewrite the same SQL, causing duplication and drift.
- No clear lineage from raw data → metric shown in dashboards.

### Semantic Views Solution
- Centralize definitions of metrics and dimensions.
- Guarantee consistency across teams/tools.
- Enforce governance via RBAC and version control.
- Enable reuse in BI, APIs, and AI applications.

---

## 🔑 Benefits
- **Consistency**: Every team uses the same definition of "Revenue" or "Active User".
- **Simplicity**: Analysts query `select * from semantic.view_sales` instead of writing complex joins.
- **Governance**: Centralized control of metric logic, role-based access.
- **Productivity**: Reduced duplication, faster delivery of dashboards and insights.

---

## 📘 Example Use Case
- Marketing defines **Customer Lifetime Value (CLV)** in Semantic Views.
- Product defines **Daily Active Users (DAU)** in the same layer.
- Finance queries both metrics from one source — no SQL rewrites.

---

Next: [Setup](./3-setup.md)
