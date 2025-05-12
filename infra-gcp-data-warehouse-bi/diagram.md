# Data Warehouse and BI on GCP

```mermaid
graph LR
    subgraph "Data Sources"
        SourceA["Source A"]
        SourceB["Source B"]
    end

    subgraph "Ingestion"
        CloudStorage["Cloud Storage\n(Landing Zone)"]
        Dataflow["Dataflow\n(Streaming/Batch ETL)"]
        DataFusion["Data Fusion\n(ETL/ELT)"]
    end

    subgraph "Storage & Processing"
        BigQuery["BigQuery\n(Data Warehouse)"]
        Dataproc["Dataproc\n(Spark/Hadoop)"]
    end

    subgraph "Business Intelligence & Analytics"
        LookerStudio["Looker Studio\n(Dashboards)"]
        Looker["Looker\n(BI Platform)"]
        VertexAI["Vertex AI\n(ML)"]
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
