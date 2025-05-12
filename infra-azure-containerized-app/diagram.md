# Containerized Application on Azure

```mermaid
graph LR
    subgraph "Client Layer"
        User["End User"]
    end

    subgraph "Network & Edge"
        AzureDNS["Azure DNS"]
        FrontDoor["Azure Front Door (Global Load Balancing/CDN)"]
        AppGateway["Application Gateway (WAF, Load Balancing)"]
    end

    subgraph "Compute & Orchestration"
        AKS["Azure Kubernetes Service (AKS)"]
        ContainerRegistry["Azure Container Registry (ACR)"]
    end

    subgraph "Data Layer"
        AzureDatabase["Azure Database for PostgreSQL/MySQL"]
        AzureCache["Azure Cache for Redis"]
        StorageAccount["Azure Storage Account (Blobs, Files)"]
    end

    subgraph "Monitoring & Security"
        AzureMonitor["Azure Monitor"]
        KeyVault["Azure Key Vault (Secrets)"]
        Defender["Defender for Cloud"]
    end

    User --> AzureDNS
    AzureDNS --> FrontDoor
    FrontDoor --> AppGateway
    AppGateway --> AKS
    AKS --> ContainerRegistry
    AKS --> AzureDatabase
    AKS --> AzureCache
    AKS --> StorageAccount

    AKS --> AzureMonitor
    AppGateway --> AzureMonitor
    AzureDatabase --> AzureMonitor
    AzureCache --> AzureMonitor

    AKS --> KeyVault
    AppGateway --> KeyVault
    AKS --> Defender
