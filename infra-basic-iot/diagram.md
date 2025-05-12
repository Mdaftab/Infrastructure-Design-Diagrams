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
        ServerlessFunctions["Serverless Functions\n(e.g., Lambda, Azure Functions, Cloud Functions)"]
        StreamProcessing["Stream Processing\n(e.g., Kinesis Analytics, Stream Analytics)"]
    end

    subgraph "Storage"
        Database["Database\n(e.g., DynamoDB, Cosmos DB, Timestream)"]
        DataLake["Data Lake\n(e.g., S3, ADLS, Cloud Storage)"]
    end

    subgraph "Monitoring & Visualization"
        MonitoringTools["Monitoring Tools\n(e.g., CloudWatch, Azure Monitor, Cloud Monitoring)"]
        Dashboard["Dashboard\n(e.g., Grafana, QuickSight, Power BI)"]
        Alerting["Alerting\n(e.g., SNS, Action Groups)"]
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
