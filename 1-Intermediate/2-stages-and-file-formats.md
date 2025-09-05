# Stages and File Formats in Snowflake

Stages and file formats are two essential components in Snowflake’s data loading and unloading workflow.  
Understanding them will help you design efficient, reusable, and scalable pipelines.

---

## 1. What is a Stage?

A **stage** is a storage location for files before they are loaded into Snowflake (or after they are unloaded).  
Think of it as a **landing zone** for your data.

### Types of Stages
1. **User Stage**
   - Automatically created for every user.  
   - Accessed via `@~`.  
   - Temporary and private to that user.  

2. **Table Stage**
   - Automatically created for every table.  
   - Accessed via `@%table_name`.  
   - Stores files intended for that specific table.  

3. **Internal Named Stage**
   - Explicitly created and managed by the user.  
   - Accessed via `@stage_name`.  
   - Can be reused across multiple tables.  

4. **External Stage**
   - References cloud storage (e.g., S3, Azure Blob, GCS).  
   - Requires credentials or integration objects.  
   - Accessed via `@stage_name` pointing to external storage.  

📌 Example:
```sql
-- Create an internal named stage
CREATE STAGE my_stage;

-- Create an external stage pointing to S3
CREATE STAGE my_s3_stage
URL='s3://my-bucket/data/'
CREDENTIALS=(AWS_KEY_ID='...' AWS_SECRET_KEY='...');
```

---

## 2. File Formats

A **file format** defines how Snowflake should interpret files when loading or unloading.  
Instead of redefining options each time, you can create reusable file format objects.

### Supported File Types
- CSV  
- JSON  
- Avro  
- ORC  
- Parquet  
- XML (beta feature in some regions)  

### Creating a File Format
```sql
CREATE FILE FORMAT my_csv_format
TYPE = 'CSV'
FIELD_DELIMITER = ','
SKIP_HEADER = 1
NULL_IF = ('NULL', 'null');
```

### Using File Formats
When loading data:
```sql
COPY INTO customers
FROM @my_stage/customers.csv
FILE_FORMAT = (FORMAT_NAME = my_csv_format);
```

When unloading data:
```sql
COPY INTO @my_stage/export/
FROM customers
FILE_FORMAT = (TYPE = 'CSV' FIELD_DELIMITER = '|');
```

---

## 3. Temporary vs. Permanent Objects

- **Temporary Stages/File Formats**
  - Exist only for the duration of a session.  
  - Useful for testing.  

- **Permanent Stages/File Formats**
  - Persist until explicitly dropped.  
  - Recommended for production pipelines.  

---

## 4. Best Practices

- Use **named stages** for reusable pipelines.  
- Create **standardized file formats** across teams for consistency.  
- Compress large files (e.g., gzip, parquet) before loading to save space and time.  
- Use **partitioned folders** in external stages (e.g., S3) for efficient data ingestion.  

---

## 📌 Key Takeaway
Stages act as the **gateway** for loading and unloading data in Snowflake, while file formats define **how the data is read or written**.  
Together, they make data ingestion pipelines **modular, reusable, and efficient**.
