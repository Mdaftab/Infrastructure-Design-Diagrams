# Microservices Architecture with API Gateway

```mermaid
graph LR
    subgraph "Clients"
        User["End User"]
        MobileApp["Mobile App"]
        WebApp["Web App"]
    end

    subgraph "API Gateway Layer"
        APIGateway["API Gateway\n(Routing, Auth, Rate Limiting)"]
    end

    subgraph "Microservices"
        ServiceA["Service A\n(e.g., User Service)"]
        ServiceB["Service B\n(e.g., Product Service)"]
        ServiceC["Service C\n(e.g., Order Service)"]
    end

    subgraph "Data Stores"
        DB_A["Database A\n(for Service A)"]
        DB_B["Database B\n(for Service B)"]
        DB_C["Database C\n(for Service C)"]
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
