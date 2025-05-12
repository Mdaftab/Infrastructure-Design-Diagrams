# Argo CD GitOps Deployment Approach

```mermaid
graph LR
    subgraph "Development Workflow"
        Developer["Developer"]
        CodeCommit["Code Commit\n(Application Code)"]
        InfraCommit["Infra Commit\n(Kubernetes Manifests)"]
    end

    subgraph "Git Repositories"
        AppRepo["Application Code Repo"]
        InfraRepo["Kubernetes Manifests Repo\n(Desired State)"]
    end

    subgraph "CI Pipeline"
        CI["CI Pipeline\n(Build, Test, Image Push)"]
        ContainerRegistry["Container Registry"]
    end

    subgraph "GitOps Controller"
        ArgoCD["Argo CD\n(GitOps Controller)"]
        ArgoCDAPI["Argo CD API/UI"]
    end

    subgraph "Kubernetes Cluster"
        K8sAPI["Kubernetes API Server"]
        K8sNodes["Kubernetes Nodes"]
        Applications["Running Applications\n(Actual State)"]
    end

    subgraph "Observability & Monitoring"
        Monitoring["Monitoring\n(Metrics, Logs, Traces)"]
        Alerting["Alerting"]
    end

    Developer --> CodeCommit
    Developer --> InfraCommit

    CodeCommit --> AppRepo
    InfraCommit --> InfraRepo

    AppRepo --> CI
    CI --> ContainerRegistry

    InfraRepo --> ArgoCD
    ContainerRegistry --> ArgoCD

    ArgoCD --> K8sAPI
    K8sAPI --> K8sNodes
    K8sNodes --> Applications

    ArgoCD --> ArgoCDAPI
    ArgoCDAPI --> Developer

    Applications --> Monitoring
    Monitoring --> Alerting
    Alerting --> Developer

    %% Sync/Reconciliation Flow
    ArgoCD -- "Monitors" --> InfraRepo
    ArgoCD -- "Compares" --> K8sAPI
    ArgoCD -- "Syncs" --> K8sAPI
    K8sAPI -- "Reports Status" --> ArgoCD
