# E-commerce Real-time Inventory & Analytics System

```mermaid
graph TB
    subgraph "CI/CD Pipeline"
        GH["GitHub Repository"]
        Actions["GitHub Actions"]
        ArgoCD["ArgoCD"]
        %% Added GitOps and security tools
        Flux["Flux"]
        Harbor["Harbor Registry"]
        Trivy["Trivy Scanner"]
        SigStore["Sigstore/Cosign"]
    end

    subgraph "GCP Infrastructure"
        subgraph "Frontend Services"
            CloudCDN["Cloud CDN"]
            CloudLB["Cloud Load Balancer"]
            GKE_FE["GKE Cluster (Frontend)"]
            %% Added feature flag service
            LaunchDarkly["LaunchDarkly"]
            %% Added WAF
            CloudArmor["Cloud Armor"]
        end
        
        subgraph "Backend Services"
            GKE_BE["GKE Cluster (Backend)"]
            %% Added service mesh
            ServiceMesh["Istio Service Mesh"]
            %% Added API gateway
            ApigeeGateway["Apigee API Gateway"]
            Cloud_SQL["Cloud SQL (PostgreSQL)"]
            Memorystore["Memorystore (Redis)"]
            Spanner["Spanner (Global)"]
        end
        
        subgraph "Data Pipeline"
            Pub_Sub["Pub/Sub"]
            Dataflow["Dataflow"]
            BigQuery["BigQuery"]
            DataStudio["Looker Studio"]
            %% Added data catalog & processing
            DataCatalog["Data Catalog"]
            Dataproc["Dataproc"]
            DLP["DLP (Data Loss Prevention)"]
        end
        
        subgraph "Security & Monitoring"
            IAM["IAM & Secret Manager"]
            CloudArmor["Cloud Armor"]
            Monitoring["Cloud Monitoring"]
            Logging["Cloud Logging"]
            %% Added security & monitoring tools
            SecurityCommand["Security Command Center"]
            BinaryAuth["Binary Authorization"]
            KMS["Cloud KMS"]
            VPC_SC["VPC Service Controls"]
        end
        
        %% Added SRE components
        subgraph "SRE & Reliability"
            SLOs["SLO Definitions"]
            ErrorBudgets["Error Budgets"]
            ChaosMonkey["Chaos Monkey"]
            Synthetics["Synthetic Monitoring"]
            OpenTelemetry["OpenTelemetry Collector"]
            Tracing["Cloud Trace"]
        end
    end
    
    subgraph "Terraform Infrastructure"
        TF_Modules["Terraform Modules"]
        TF_State["Cloud Storage (TF State)"]
        %% Added Terraform enhancements
        TF_Cloud["Terraform Cloud"]
        Atlantis["Atlantis"]
        Checkov["Checkov (Policy)"]
        Sentinel["Sentinel Policies"]
    end
    
    %% Connections
    GH --> Actions
    Actions --> TF_Modules
    Actions --> Trivy
    Actions --> SigStore
    Actions --> Harbor
    
    Harbor --> GKE_FE
    Harbor --> GKE_BE
    
    TF_Modules --> GCP_Infrastructure
    TF_Modules --> TF_State
    TF_Modules --> TF_Cloud
    TF_Cloud --> Sentinel
    Checkov --> TF_Modules
    
    Actions --> ArgoCD & Flux
    ArgoCD --> GKE_FE
    ArgoCD --> GKE_BE
    Flux --> GKE_FE
    Flux --> GKE_BE
    
    CloudArmor --> CloudLB
    CloudCDN --> CloudLB
    CloudLB --> GKE_FE
    
    LaunchDarkly -.-> GKE_FE
    
    GKE_FE --> ApigeeGateway
    ApigeeGateway --> GKE_BE
    
    ServiceMesh -.- GKE_BE
    
    GKE_BE --> Cloud_SQL
    GKE_BE --> Memorystore
    GKE_BE --> Spanner
    GKE_BE --> Pub_Sub
    
    Pub_Sub --> Dataflow
    Pub_Sub --> Dataproc
    Dataflow --> BigQuery
    Dataproc --> BigQuery
    BigQuery --> DataStudio
    BigQuery --> DataCatalog
    
    DLP -.-> BigQuery
    DLP -.-> Pub_Sub
    
    IAM -.-> GCP_Infrastructure
    SecurityCommand -.-> GCP_Infrastructure
    BinaryAuth -.-> GKE_FE & GKE_BE
    KMS -.-> GCP_Infrastructure
    VPC_SC -.-> GCP_Infrastructure
    
    Monitoring --> SLOs
    SLOs --> ErrorBudgets
    OpenTelemetry --> Monitoring & Tracing & Logging
    Synthetics --> SLOs
    ChaosMonkey --> GKE_BE & GKE_FE
    
    Monitoring -.-> GCP_Infrastructure
    Logging -.-> GCP_Infrastructure
```