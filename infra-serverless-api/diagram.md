# Serverless API with Database

```mermaid
graph LR
    subgraph "Client Layer"
        User["End User"]
    end

    subgraph "API Layer"
        APIGateway["API Gateway\n(e.g., API Gateway, Azure API Management)"]
    end

    subgraph "Compute Layer"
        ServerlessFunctions["Serverless Functions\n(e.g., Lambda, Azure Functions, Cloud Functions)"]
    end

    subgraph "Data Layer"
        Database["Database\n(e.g., DynamoDB, Cosmos DB, Cloud SQL)"]
    end

    User --> APIGateway
    APIGateway --> ServerlessFunctions
    ServerlessFunctions --> Database
