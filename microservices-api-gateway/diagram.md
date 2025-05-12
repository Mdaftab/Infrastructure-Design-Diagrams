# Microservices Architecture with API Gateway

```mermaid
graph LR
    subgraph "Clients"
        User["End User"]
        MobileApp["Mobile App"]
        WebApp["Web App"]
    end

    subgraph "API Gateway Layer"
        APIGateway["API Gateway (Routing, Auth, Rate Limiting)"]
    end

    subgraph "Microservices"
        ServiceA["Service A (e.g., User Service)"]
        ServiceB["Service B (e.g., Product Service)"]
        ServiceC["Service C (e.g., Order Service)"]
    end

    subgraph "Data Stores"
        DB_A["Database A (for Service A)"]
        DB_B["Database B (for Service B)"]
        DB_C["Database C (for Service C)"]
    end

    User --> APIGateway
    MobileApp --> APIGateway
    WebApp --> APIGateway

    APIGateway --> ServiceA
    APIGateway --> ServiceB
    APIGateway --> ServiceC

    ServiceA --> DB_A
    ServiceB --> DB_B
    ServiceC --> DB_C
