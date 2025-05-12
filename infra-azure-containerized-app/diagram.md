# Containerized Application on Azure

```mermaid
graph LR
    subgraph "Client Layer"
        User["End User"]
    end

    subgraph "Network & Edge"
        AzureDNS["Azure DNS"]
        FrontDoor["Azure Front Door\n(Global Load Balancing/CDN)"]
        AppGateway["Application Gateway\n(WAF, Load Balancing)"]
    end

    subgraph "Compute & Orchestration"
        AKS["Azure Kubernetes Service\n(AKS)"]
        ContainerRegistry["Azure Container Registry\n(ACR)"]
    end

    subgraph "Data Layer"
        AzureDatabase["Azure Database for PostgreSQL/MySQL"]
        AzureCache["Azure Cache for Redis"]
        StorageAccount["Azure Storage Account\n(Blobs, Files)"]
    end

    subgraph "Monitoring & Security"
        AzureMonitor["Azure Monitor"]
        KeyVault["Azure Key Vault\n(Secrets)"]
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
