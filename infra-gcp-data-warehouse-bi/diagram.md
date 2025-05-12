# Data Warehouse and BI on GCP

```mermaid
graph LR
    subgraph "Data Sources"
        SourceA["Source A"]
        SourceB["Source B"]
    end

    subgraph "Ingestion"
        CloudStorage["Cloud Storage (Landing Zone)"]
        Dataflow["Dataflow (Streaming/Batch ETL)"]
        DataFusion["Data Fusion (ETL/ELT)"]
    end

    subgraph "Storage & Processing"
        BigQuery["BigQuery (Data Warehouse)"]
        Dataproc["Dataproc (Spark/Hadoop)"]
    end

    subgraph "Business Intelligence & Analytics"
        LookerStudio["Looker Studio (Dashboards)"]
        Looker["Looker (BI Platform)"]
        VertexAI["Vertex AI (ML)"]
    end

    SourceA --> CloudStorage
    SourceB --> DataFusion

    CloudStorage --> Dataflow
    CloudStorage --> Dataproc
    Dataflow --> BigQuery
    DataFusion --> BigQuery
    Dataproc --> BigQuery

    BigQuery --> LookerStudio
    BigQuery --> Looker
    BigQuery --> VertexAI
