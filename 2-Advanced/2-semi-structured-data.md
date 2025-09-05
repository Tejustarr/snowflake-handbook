# Semi-Structured Data in Snowflake

Traditional relational databases are built for structured, tabular data.  
Snowflake, however, provides **native support for semi-structured formats** like JSON, Avro, ORC, and Parquet.  
This allows analysts to query complex, nested data using familiar SQL.

---

## 1. The VARIANT Data Type

- Semi-structured data in Snowflake is stored in the **VARIANT** type.  
- A `VARIANT` column can hold values of different types (string, number, object, array).  
- Snowflake automatically parses the data into an internal optimized format.  

Example:
```sql
CREATE TABLE raw_orders (
    order_id INT,
    data VARIANT
);

-- Insert JSON into the VARIANT column
INSERT INTO raw_orders VALUES
(1, PARSE_JSON('{"customer_id": 101, "items": [{"product": "Book", "price": 15}]}'));
```

---

## 2. Querying JSON Data

You can query nested attributes using the **colon (`:`) operator**.

```sql
SELECT 
    data:customer_id AS customer,
    data:items[0].product AS first_item,
    data:items[0].price::NUMBER AS first_item_price
FROM raw_orders;
```

- `data:customer_id` → Accesses a JSON field.  
- `data:items[0].product` → Accesses an array element.  
- `::NUMBER` → Casts the value into a numeric type.  

---

## 3. Working with Arrays

Snowflake provides functions to handle arrays inside JSON.

Example: Exploding an array into multiple rows
```sql
SELECT 
    order_id,
    item.value:product AS product,
    item.value:price::NUMBER AS price
FROM raw_orders,
LATERAL FLATTEN(input => data:items) item;
```

📌 The `FLATTEN` function expands array elements into separate rows.  

---

## 4. Other Supported Formats

Besides JSON, Snowflake natively supports:

- **Avro**  
- **ORC**  
- **Parquet**  

You can load these formats directly into VARIANT columns.

Example with Parquet:
```sql
CREATE STAGE parquet_stage
URL='s3://mybucket/data/';

COPY INTO raw_data
FROM @parquet_stage
FILE_FORMAT = (TYPE = 'PARQUET');
```

---

## 5. Semi-Structured Functions

Snowflake provides a rich set of functions to manipulate semi-structured data:

- `OBJECT_KEYS()` → Returns the keys in a JSON object.  
- `ARRAY_SIZE()` → Returns the length of an array.  
- `GET_PATH()` → Retrieves a value at a specified JSON path.  
- `TRY_PARSE_JSON()` → Safely parses JSON without throwing errors.  

Example:
```sql
SELECT 
    OBJECT_KEYS(data) AS keys,
    ARRAY_SIZE(data:items) AS total_items
FROM raw_orders;
```

---

## 6. When to Use VARIANT vs. Relational Tables

- Use **VARIANT** when:
  - The schema is flexible or frequently changing.  
  - You need to ingest raw JSON/Parquet quickly.  
  - You want to prototype fast without modeling the data upfront.  

- Use **relational columns** when:
  - The schema is stable and well-defined.  
  - You need strict types for reporting/BI tools.  
  - Performance optimization is critical.  

---

## 📌 Key Takeaway
Snowflake’s **VARIANT data type** and functions make it easy to work with semi-structured data.  
You can **store, query, and transform JSON, Avro, Parquet, and ORC** directly with SQL,  
bridging the gap between raw data lakes and structured warehouses.
