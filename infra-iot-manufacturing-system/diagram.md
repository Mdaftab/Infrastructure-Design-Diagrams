# IoT-Based Smart Manufacturing System

```mermaid
flowchart TB
    subgraph "Factory Floor"
        Sensors["IoT Sensors"]
        Gateways["Edge Gateways"]
        Controllers["PLC Controllers"]
    end
    
    subgraph "Edge Computing"
        EdgeDevices["Edge Devices"]
        LocalProcessing["Local Processing Units"]
        EdgeML["Edge ML Inference"]
    end
    
    subgraph "AWS Cloud Infrastructure"
        subgraph "IoT & Ingestion"
            IoTCore["AWS IoT Core"]
            Greengrass["AWS Greengrass"]
            IoTAnalytics["IoT Analytics"]
        end
        
        subgraph "Data Processing"
            Kinesis["Kinesis Data Streams"]
            KinesisAnalytics["Kinesis Data Analytics"]
            Lambda["Lambda Functions"]
            StepFunctions["Step Functions"]
        end
        
        subgraph "Storage & Database"
            Timestream["Timestream (Time Series)"]
            S3["S3 (Data Lake)"]
            DynamoDB["DynamoDB"]
            RDS["RDS PostgreSQL"]
        end
        
        subgraph "Machine Learning"
            SageMaker["SageMaker"]
            ComprehendCustom["Comprehend Custom"]
            Forecast["Forecast"]
        end
        
        subgraph "Application Layer"
            EKS["EKS Cluster"]
            ECS["ECS Services"]
            AppMesh["App Mesh"]
        end
        
        subgraph "Visualization & Alerting"
            QuickSight["QuickSight"]
            Grafana["Managed Grafana"]
            SNS["SNS"]
            EventBridge["EventBridge"]
        end
    end
    
    subgraph "DevOps & Security"
        CodePipeline["CodePipeline"]
        TerraformCloud["Terraform Cloud"]
        Config["AWS Config"]
        SecurityHub["Security Hub"]
        NetworkFirewall["Network Firewall"]
    end
    
    %% Connections
    Sensors --> Gateways
    Gateways --> EdgeDevices
    Controllers --> LocalProcessing
    
    EdgeDevices --> EdgeML
    EdgeDevices & LocalProcessing --> Greengrass
    
    Greengrass --> IoTCore
    IoTCore --> Kinesis
    IoTCore --> IoTAnalytics
    
    Kinesis --> KinesisAnalytics
    Kinesis --> Lambda
    KinesisAnalytics --> Lambda
    Lambda --> StepFunctions
    
    IoTAnalytics & Kinesis --> Timestream
    IoTAnalytics & Kinesis --> S3
    Lambda --> DynamoDB
    Lambda --> RDS
    
    S3 --> SageMaker
    Timestream --> Forecast
    S3 --> ComprehendCustom
    
    SageMaker --> EKS
    Lambda --> ECS
    AppMesh --> EKS & ECS
    
    DynamoDB & RDS & Timestream --> QuickSight
    Timestream --> Grafana
    
    EventBridge --> SNS
    Lambda --> EventBridge
    
    CodePipeline --> TerraformCloud
    TerraformCloud --> AWS_Cloud_Infrastructure
    
    Config & SecurityHub & NetworkFirewall -.-> AWS_Cloud_Infrastructure
    NetworkFirewall -.-> IoTCore
```