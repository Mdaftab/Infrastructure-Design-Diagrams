# Serverless Web App on AWS

```mermaid
graph LR
    subgraph "Client Layer"
        User["End User"]
    end

    subgraph "Edge & Presentation"
        CloudFront["CloudFront\n(CDN)"]
        S3Hosting["S3\n(Static Website Hosting)"]
    end

    subgraph "API & Compute"
        APIGateway["API Gateway\n(REST/HTTP API)"]
        Lambda["Lambda\n(Backend Logic)"]
    end

    subgraph "Data Layer"
        DynamoDB["DynamoDB\n(NoSQL Database)"]
        S3Data["S3\n(Data Storage)"]
    end

    User --> CloudFront
    CloudFront --> S3Hosting
    CloudFront --> APIGateway
    APIGateway --> Lambda
    Lambda --> DynamoDB
    Lambda --> S3Data
