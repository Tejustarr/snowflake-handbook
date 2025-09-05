# Tasks, Streams, and Pipes in Snowflake

Snowflake is not only a data warehouse but also a **data engineering platform**.  
It provides built-in features to build pipelines for **continuous ingestion, transformations, and scheduling** — without external tools.

The three key components are **Streams, Tasks, and Pipes**.

---

## 1. Streams

A **stream** is a change-tracking object that records modifications (INSERTs, UPDATEs, DELETEs) made to a table.

- Acts like a **CDC (Change Data Capture)** mechanism.  
- Stores metadata about what changed, not the data itself.  
- Useful for incremental ETL processes.  

Example:
```sql
-- Create a stream to track changes in the orders table
CREATE OR REPLACE STREAM orders_stream ON TABLE orders;

-- Query the stream
SELECT * FROM orders_stream;
```

📌 Each record shows the type of change (`INSERT`, `DELETE`, `UPDATE`) and the affected row.

---

## 2. Tasks

A **task** is a scheduled or dependency-based SQL job in Snowflake.

- Can run queries, transformations, or procedural logic.  
- Supports **cron-like schedules** or dependency chaining.  
- Runs inside a warehouse.  

Example:
```sql
-- Create a task that runs every hour
CREATE OR REPLACE TASK hourly_task
WAREHOUSE = my_wh
SCHEDULE = 'USING CRON 0 * * * * UTC'
AS
INSERT INTO daily_sales_summary
SELECT customer_id, SUM(amount) AS total
FROM orders_stream
GROUP BY customer_id;

-- Start the task
ALTER TASK hourly_task RESUME;
```

---

## 3. Pipes (Snowpipe)

A **pipe** is an object that defines **continuous data ingestion** from a stage into a table.

- Automates the `COPY INTO` command.  
- Works with **Snowpipe**, which listens for new files in a stage.  
- Supports event notifications from cloud storage (e.g., S3 events).  

Example:
```sql
CREATE OR REPLACE PIPE orders_pipe
AS
COPY INTO orders
FROM @orders_stage
FILE_FORMAT = (TYPE = 'CSV');
```

Once configured, Snowpipe automatically loads new files as soon as they arrive.

---

## 4. How They Work Together

- **Streams** → Track changes in data.  
- **Tasks** → Schedule and automate transformations.  
- **Pipes (Snowpipe)** → Continuously load new files from cloud storage.  

📌 Together, they enable **end-to-end ELT pipelines** directly inside Snowflake.

---

## 5. Use Cases

- **Real-time ingestion** of logs or events from S3.  
- **Incremental ETL** pipelines using streams + tasks.  
- **Automated dashboards** that refresh continuously.  
- **Data lake ingestion** into Snowflake tables.  

---

## 6. Best Practices

- Use **small, frequent files** for Snowpipe (better parallelism).  
- Monitor **latency and costs** of continuous ingestion.  
- Use **resource monitors** to control warehouse usage for tasks.  
- Drop unused streams and tasks to reduce metadata overhead.  

---

## 📌 Key Takeaway
- **Streams** → Track data changes.  
- **Tasks** → Automate transformations with scheduling.  
- **Pipes (Snowpipe)** → Handle continuous data ingestion.  

Together, these features let you build **fully managed pipelines in Snowflake** without relying on external orchestration tools.
