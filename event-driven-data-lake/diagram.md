# Event-Driven Data Lake Architecture

```mermaid
graph LR
    subgraph "Data Sources"
        SourceA["Source A\n(e.g., Applications, Devices)"]
        SourceB["Source B\n(e.g., Databases, APIs)"]
    end

    subgraph "Ingestion Layer"
        StreamingService["Streaming Service (e.g., Kinesis, Event Hubs, Pub/Sub)"]
        BatchIngestion["Batch Ingestion (e.g., DataSync, Data Transfer)"]
    end

    subgraph "Storage Layer"
        RawDataLake["Raw Data Lake (e.g., S3, ADLS Gen2, Cloud Storage)"]
        ProcessedDataLake["Processed Data Lake (e.g., S3, ADLS Gen2, Cloud Storage)"]
    end

    subgraph "Processing Layer"
        StreamProcessing["Stream Processing (e.g., Kinesis Analytics, Stream Analytics, Dataflow)"]
        BatchProcessing["Batch Processing (e.g., EMR, Databricks, Dataproc)"]
        ETL["ETL/ELT Tools (e.g., Glue, Data Factory, Data Fusion)"]
    end

    subgraph "Consumption Layer"
        DataWarehouse["Data Warehouse (e.g., Redshift, Synapse Analytics, BigQuery)"]
        AnalyticsTools["Analytics Tools (e.g., QuickSight, Power BI, Looker)"]
        MLServices["ML Services (e.g., SageMaker, Azure ML, Vertex AI)"]
    end

    SourceA --> StreamingService
    SourceB --> BatchIngestion

    StreamingService --> RawDataLake
    BatchIngestion --> RawDataLake

    RawDataLake --> StreamProcessing
    RawDataLake --> BatchProcessing
    RawDataLake --> ETL

    StreamProcessing --> ProcessedDataLake
    BatchProcessing --> ProcessedDataLake
    ETL --> ProcessedDataLake

    ProcessedDataLake --> DataWarehouse
    ProcessedDataLake --> AnalyticsTools
    ProcessedDataLake --> MLServices
    DataWarehouse --> AnalyticsTools
    DataWarehouse --> MLServices
