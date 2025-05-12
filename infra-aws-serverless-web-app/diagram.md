# Serverless Web App on AWS

```mermaid
graph LR
    subgraph "Client Layer"
        User["End User"]
    end

    subgraph "Edge & Presentation"
        CloudFront["CloudFront (CDN)"]
        S3Hosting["S3 (Static Website Hosting)"]
    end

    subgraph "API & Compute"
        APIGateway["API Gateway (REST/HTTP API)"]
        Lambda["Lambda (Backend Logic)"]
    end

    subgraph "Data Layer"
        DynamoDB["DynamoDB (NoSQL Database)"]
        S3Data["S3 (Data Storage)"]
    end

    User --> CloudFront
    CloudFront --> S3Hosting
    CloudFront --> APIGateway
    APIGateway --> Lambda
    Lambda --> DynamoDB
    Lambda --> S3Data
