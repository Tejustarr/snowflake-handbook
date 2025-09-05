# Openflow Diagrams

This file contains several Mermaid diagrams illustrating common Openflow pipeline patterns:
- High-level ingestion flow
- CDC (Change Data Capture) pipeline
- Streaming (Kafka → Snowflake) pipeline
- Error handling & retry flow

> **Note:** GitHub renders Mermaid in Markdown on the web UI.  
> If you open this locally, you may see the raw code unless your editor supports Mermaid.

---

## 1. High-level Data Loading Flow

```mermaid
flowchart TD
  subgraph S[Sources]
    A1["Files - S3, GCS, Blob"]
    A2["Databases - SQL Server, MySQL"]
    A3["APIs / SaaS (REST)"]
    A4["Streaming - Kafka, Kinesis"]
  end

  subgraph O[Openflow Pipeline]
    B1["Connector / Ingest"]
    B2["Processors / Transforms"]
    B3["Routing to Stage or Direct"]
    B4["Monitoring / Alerts"]
  end

  subgraph SF[Snowflake Target]
    C1["Stage (internal / external)"]
    C2["Raw Tables / Streams"]
    C3["Transform (dbt / Tasks)"]
    C4["Analytics / BI / ML"]
  end

  A1 --> B1
  A2 --> B1
  A3 --> B1
  A4 --> B1

  B1 --> B2
  B2 --> B3
  B3 --> C1
  B3 --> C2
  C1 --> C2
  C2 --> C3
  C3 --> C4

  B4 -.-> C4
```

---

## 2. CDC Pipeline (Database → Snowflake via Openflow)

```mermaid
flowchart LR
  subgraph OPDB[OLTP Database]
    D1[(CDC Stream)]
  end

  subgraph OF[Openflow]
    O1["CDC Connector"]
    O2["Parse & Normalize"]
    O3["Dedup / Upsert Logic"]
    O4["Write to Stage / Table"]
  end

  subgraph SF2[Snowflake]
    S1["Stage - db_cdc_stage"]
    S2["Stream - orders_stream"]
    S3["Table - orders"]
  end

  D1 --> O1 --> O2 --> O3 --> O4 --> S1
  S1 -->|COPY or Snowpipe| S3
  S3 --> S2
  S2 -->|MERGE| S3
```

---

## 3. Streaming Pipeline (Kafka → Openflow → Snowflake)

```mermaid
flowchart TB
  subgraph KAFKA[Kafka Cluster]
    K1[(kafka.topic.events)]
  end

  subgraph OF2[Openflow]
    Oa["Kafka Connector"]
    Ob["JSON Parser / Schema Registry"]
    Oc["Enrichment / Lookup"]
    Od["Snowflake Sink"]
  end

  subgraph SF3[Snowflake]
    T1["Stage - kafka_stage"]
    T2["Raw Table - raw_events"]
    T3["Processed - events_clean"]
  end

  K1 --> Oa --> Ob --> Oc --> Od
  Od --> T1
  T1 -->|Snowpipe| T2
  T2 --> T3
```

---

## 4. Error Handling & Retry Flow

```mermaid
flowchart TD
  I["Ingest Task"]
  X["Transform Step"]
  V["Validation"]
  S["Success / Commit"]
  RQ["Retry Queue"]
  DLQ["Dead-Letter Queue"]
  AL["Alert / Dashboard"]

  I --> X --> V
  V -->|OK| S
  V -->|Fail| RQ
  RQ -->|retry| X
  RQ -->|max retries reached| DLQ
  DLQ --> AL
```

---

## Plain-text fallback (if Mermaid doesn’t render)

- **High-level flow:**  
  Sources → Openflow (Connectors → Processors → Routing) → Snowflake (Stage → Raw Tables → Transforms) → Consumers (BI / ML)

- **CDC flow:**  
  DB CDC → Openflow → Stage → COPY/Snowpipe → Raw table → Stream + MERGE → Final table

- **Streaming flow:**  
  Kafka → Openflow → Stage → Snowpipe → Raw table → Processed table

- **Retry flow:**  
  Ingest → Transform → Validate → (OK → Commit) / (Fail → Retry queue → Dead-letter + alert)
