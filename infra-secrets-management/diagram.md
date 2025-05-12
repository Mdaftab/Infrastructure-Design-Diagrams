# Secrets Management Architecture

```mermaid
graph LR
    subgraph "Development & CI/CD"
        Developer["Developer"]
        CI_CD_Pipeline["CI/CD Pipeline"]
        CodeRepository["Code Repository"]
        ConfigFiles["Configuration Files\n(No Secrets)"]
    end

    subgraph "Secrets Management System"
        SecretsManager["Secrets Manager\n(e.g., Vault, AWS Secrets Manager, Azure Key Vault)"]
        AuditLogs["Audit Logs"]
        EncryptionKeys["Encryption Keys\n(KMS)"]
    end

    subgraph "Runtime Environment"
        Applications["Applications / Services"]
        ComputeResources["Compute Resources\n(VMs, Containers, Functions)"]
        Database["Database / External Service"]
    end

    Developer --> CodeRepository
    CodeRepository --> CI_CD_Pipeline
    CI_CD_Pipeline --> ConfigFiles
    CI_CD_Pipeline --> SecretsManager

    Applications --> SecretsManager
    ComputeResources --> Applications
    SecretsManager --> EncryptionKeys
    SecretsManager --> AuditLogs
    Applications --> Database
