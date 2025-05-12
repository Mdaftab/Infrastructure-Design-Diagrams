# Event-Driven Microservices Architecture

```mermaid
graph LR
    subgraph "Clients"
        User["End User"]
    end

    subgraph "API Gateway"
        APIGateway["API Gateway"]
    end

    subgraph "Microservices"
        ServiceA["Service A (Publishes Events)"]
        ServiceB["Service B (Subscribes to Events)"]
        ServiceC["Service C (Subscribes to Events)"]
    end

    subgraph "Messaging / Event Bus"
        EventBus["Event Bus / Message Broker (e.g., Kafka, RabbitMQ, Pub/Sub, Event Hubs)"]
    end

    subgraph "Data Stores"
        DB_A["Database A"]
        DB_B["Database B"]
        DB_C["Database C"]
    end

    User --> APIGateway
    APIGateway --> ServiceA

    ServiceA --> EventBus
    EventBus --> ServiceB
    EventBus --> ServiceC

    ServiceA --> DB_A
    ServiceB --> DB_B
    ServiceC --> DB_C
