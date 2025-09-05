# Openflow Examples

This file provides hands-on style examples to illustrate how Snowflake Openflow can be used for real-world pipelines.  
Examples include **flow design, SQL integration, and CLI usage**.

---

## 1. Basic Flow: CSV to Table

A simple pipeline to load customer data from a CSV file in S3 into a Snowflake table.

**Flow Design Steps**
1. Input → S3 Connector  
2. Transformation → Field Mapping (optional)  
3. Output → Snowflake Table  

**Equivalent SQL Target Table**
```sql
CREATE OR REPLACE TABLE customers (
    id INT,
    name STRING,
    email STRING,
    country STRING
);
```

📌 The flow automatically handles ingestion into `customers` without manual `COPY INTO`.

---

## 2. SQL Server CDC to Snowflake

Capture changes from SQL Server and apply them to a Snowflake table.

**Flow Design**
- Source: SQL Server CDC Connector  
- Transformation: Map fields  
- Sink: Snowflake Target Table  

**Pipeline Result**
```sql
-- Incoming CDC stream (INSERT/UPDATE/DELETE)
SELECT * FROM orders_stream;

-- Apply changes to target table
MERGE INTO orders t
USING orders_stream s
ON t.id = s.id
WHEN MATCHED AND s.op = 'DELETE' THEN DELETE
WHEN MATCHED THEN UPDATE SET t.amount = s.amount
WHEN NOT MATCHED THEN INSERT (id, amount) VALUES (s.id, s.amount);
```

---

## 3. REST API to Snowflake

Pull JSON data from an external API and load into a Snowflake VARIANT column.

**Flow Design**
- Source: REST API Connector (e.g., weather API)  
- Transformation: None (raw JSON)  
- Sink: Snowflake table  

**Target Table**
```sql
CREATE OR REPLACE TABLE weather_data (
    id INT AUTOINCREMENT,
    payload VARIANT,
    load_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

📌 The flow sends each JSON payload directly into `payload`.

---

## 4. Using Openflow with dbt

Integrate Openflow ingestion with **dbt transformations**.

1. **Ingest raw data** into a staging table:
```sql
CREATE OR REPLACE TABLE raw_sales AS
SELECT * FROM openflow_sales_stage;
```

2. **Transform with dbt model**:
```sql
-- dbt model: sales_cleaned.sql
SELECT 
    id,
    CAST(order_date AS DATE) AS order_date,
    amount::NUMBER AS amount
FROM raw_sales;
```

3. **Expose transformed data** to BI tools.

---

## 5. CLI Example

Deploy an Openflow pipeline using Snowflake CLI (pseudo example):

```bash
snowflow deploy \
  --flow-file order_ingest.json \
  --target snowflake_db.analytics.orders \
  --schedule "0 * * * *"
```

This deploys a flow defined in `order_ingest.json` and schedules it to run every hour.

---

## 📌 Key Takeaway

- Openflow pipelines can ingest from **files, databases, APIs, or streams**.  
- They integrate smoothly with **SQL + dbt** for transformations.  
- Deployment can be done via UI or CLI, making automation easier.
