# Microservices Architecture with Service Mesh

```mermaid
graph LR
    subgraph "Clients"
        User["End User"]
    end

    subgraph "API Gateway"
        APIGateway["API Gateway"]
    end

    subgraph "Microservices with Service Mesh"
        subgraph "Service A Pod"
            ServiceA["Service A"]
            ProxyA["Envoy Proxy"]
        end
        subgraph "Service B Pod"
            ServiceB["Service B"]
            ProxyB["Envoy Proxy"]
        end
        subgraph "Service C Pod"
            ServiceC["Service C"]
            ProxyC["Envoy Proxy"]
        end
    end

    subgraph "Service Mesh Control Plane"
        ControlPlane["Control Plane (e.g., Istio, Linkerd, App Mesh)"]
        ObservabilityTools["Observability Tools (Metrics, Tracing, Logging)"]
    end

    subgraph "Data Stores"
        DB_A["Database A"]
        DB_B["Database B"]
        DB_C["Database C"]
    end

    User --> APIGateway
    APIGateway --> ProxyA
    APIGateway --> ProxyB
    APIGateway --> ProxyC

    ProxyA <--> ServiceA
    ProxyB <--> ServiceB
    ProxyC <--> ServiceC

    ProxyA --> ProxyB
    ProxyA --> ProxyC
    ProxyB --> ProxyA
    ProxyB --> ProxyC
    ProxyC --> ProxyA
    ProxyC --> ProxyB

    ControlPlane --> ProxyA
    ControlPlane --> ProxyB
    ControlPlane --> ProxyC

    ProxyA --> ObservabilityTools
    ProxyB --> ObservabilityTools
    ProxyC --> ObservabilityTools

    ServiceA --> DB_A
    ServiceB --> DB_B
    ServiceC --> DB_C
