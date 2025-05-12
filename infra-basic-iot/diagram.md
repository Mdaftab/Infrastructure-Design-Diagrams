# Basic IoT Data Ingestion and Monitoring System

```mermaid
graph LR
    subgraph "IoT Devices"
        DeviceA["IoT Device A"]
        DeviceB["IoT Device B"]
        ...
        DeviceN["IoT Device N"]
    end

    subgraph "Connectivity & Ingestion"
        IoTHub["IoT Hub / IoT Core / Pub/Sub"]
    end

    subgraph "Processing"
        ServerlessFunctions["Serverless Functions (e.g., Lambda, Azure Functions, Cloud Functions)"]
        StreamProcessing["Stream Processing (e.g., Kinesis Analytics, Stream Analytics)"]
    end

    subgraph "Storage"
        Database["Database (e.g., DynamoDB, Cosmos DB, Timestream)"]
        DataLake["Data Lake (e.g., S3, ADLS, Cloud Storage)"]
    end

    subgraph "Monitoring & Visualization"
        MonitoringTools["Monitoring Tools (e.g., CloudWatch, Azure Monitor, Cloud Monitoring)"]
        Dashboard["Dashboard (e.g., Grafana, QuickSight, Power BI)"]
        Alerting["Alerting (e.g., SNS, Action Groups)"]
    end

    DeviceA --> IoTHub
    DeviceB --> IoTHub
    DeviceN --> IoTHub

    IoTHub --> ServerlessFunctions
    IoTHub --> StreamProcessing

    ServerlessFunctions --> Database
    ServerlessFunctions --> DataLake
    StreamProcessing --> Database
    StreamProcessing --> DataLake

    Database --> MonitoringTools
    DataLake --> MonitoringTools
    MonitoringTools --> Dashboard
    MonitoringTools --> Alerting
    Alerting --> MonitoringTools
