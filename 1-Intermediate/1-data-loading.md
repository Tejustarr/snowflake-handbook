# Data Loading in Snowflake

Getting data into Snowflake is a critical first step in any analytics workflow.  
Snowflake provides flexible ways to load data from files, cloud storage, or external systems.

---

## 1. Loading Options in Snowflake

Snowflake supports multiple methods for loading data:

1. **Web UI (Snowsight)**  
   - Drag-and-drop files from your local machine.  
   - Good for small test datasets or quick demos.  

2. **SnowSQL (CLI)**  
   - Command-line tool for executing SQL commands.  
   - Useful for automation and scripting.  

3. **Stages + COPY INTO (Recommended)**  
   - Upload data into a **stage** (internal or external).  
   - Use `COPY INTO` to load data into tables.  
   - Handles large-scale, repeatable data loading.  

4. **Third-party integrations**  
   - ETL/ELT tools like Fivetran, Airbyte, Informatica, or dbt.  
   - Stream data from Kafka or other messaging systems.  

---

## 2. What is a Stage?

A **stage** is a location where files are stored before being loaded into Snowflake.

- **Internal Stage** → Managed storage inside Snowflake.  
- **External Stage** → Points to cloud storage (S3, Azure Blob, GCS).  
- **Table Stage** → Each table has a built-in stage (`@%table_name`).  

📌 Example:
```sql
-- Create a named internal stage
CREATE STAGE mystage;

-- Put a file into the stage (via SnowSQL CLI)
PUT file://customers.csv @mystage;

-- Copy the file data into a table
COPY INTO customers
FROM @mystage/customers.csv
FILE_FORMAT = (TYPE = 'CSV' FIELD_OPTIONALLY_ENCLOSED_BY='"');
```

---

## 3. File Formats

Snowflake supports multiple formats for loading:
- CSV  
- JSON  
- Parquet  
- Avro  
- ORC  

You can **define a reusable file format** object:
```sql
CREATE FILE FORMAT my_csv_format
TYPE = 'CSV'
FIELD_DELIMITER = ','
SKIP_HEADER = 1;
```

---

## 4. COPY INTO Command

`COPY INTO` is the primary command for bulk loading data.

Example with CSV:
```sql
COPY INTO customers
FROM @mystage
FILE_FORMAT = (FORMAT_NAME = my_csv_format);
```

Example with JSON:
```sql
COPY INTO raw_orders
FROM @mystage/orders/
FILE_FORMAT = (TYPE = 'JSON');
```

---

## 5. Continuous Data Loading

For ongoing pipelines:
- **Snowpipe** → Automatically loads new files as soon as they appear in a stage.  
- Uses **event notifications** from cloud storage (e.g., S3 events).  
- Ideal for near real-time ingestion.  

---

## 6. Unloading Data

Snowflake also allows exporting data back to stages:
```sql
COPY INTO @mystage/export/
FROM customers
FILE_FORMAT = (TYPE = 'CSV' FIELD_OPTIONALLY_ENCLOSED_BY='"');
```

---

## 📌 Key Takeaway
Data loading in Snowflake revolves around **stages, file formats, and the `COPY INTO` command**.  
For real-time or continuous pipelines, **Snowpipe** provides automated ingestion.  
This flexibility allows Snowflake to handle **both batch and streaming data ingestion** with ease.
