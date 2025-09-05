# Data Loading Flow Diagram

```mermaid
flowchart TD
    subgraph Sources [Data Sources]
        A1[CSV/JSON Files]
        A2[Cloud Storage<br>(S3, Azure Blob, GCS)]
        A3[Streaming/ETL Tools]
    end

    subgraph Stage [Snowflake Stage]
        B1[Internal Stage]
        B2[External Stage]
        B3[Table Stage]
    end

    subgraph Loading [Data Loading Options]
        C1[COPY INTO<br>(Batch Load)]
        C2[Snowpipe<br>(Auto/Continuous)]
    end

    subgraph Target [Snowflake Tables]
        D1[Raw Table]
        D2[Transformed Table]
    end

    subgraph Usage [Consumption Layer]
        E1[BI Tools<br>(Tableau, Power BI)]
        E2[Analytics Queries]
        E3[ML/AI Pipelines]
    end

    %% Connections
    A1 --> Stage
    A2 --> Stage
    A3 --> Stage

    Stage --> C1
    Stage --> C2

    C1 --> D1
    C2 --> D1

    D1 --> D2
    D2 --> Usage
```

📌 This diagram shows:
- Multiple **data sources** (files, cloud storage, ETL).  
- Landing into a **stage** (internal, external, or table).  
- Data loading via **COPY INTO (batch)** or **Snowpipe (continuous)**.  
- Final storage in **tables**, then consumed by BI, analytics, or ML pipelines.
