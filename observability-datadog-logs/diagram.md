# Datadog Log Management Flow

```mermaid
graph LR
    subgraph "Applications & Infrastructure"
        AppA["Application A\n(Generates Logs)"]
        AppB["Application B\n(Generates Logs)"]
        Infra["Infrastructure\n(Hosts, Containers, Services)"]
    end

    subgraph "Datadog Agents & Integrations"
        DatadogAgent["Datadog Agent (Host/Container)"]
        Integrations["Cloud/Service Integrations (e.g., S3, CloudWatch Logs, Azure Blob Storage)"]
    end

    subgraph "Datadog Platform"
        LogIngestion["Log Ingestion (Receives Logs)"]
        LogProcessing["Log Processing Pipelines (Parsing, Enrichment, Filtering)"]
        LogIndexing["Log Indexing (Storage & Search)"]
        LogAnalytics["Log Analytics Engine"]
    end

    subgraph "Datadog UI & Features"
        LogExplorer["Log Explorer (Search & Filter)"]
        Dashboards["Dashboards (Visualization)"]
        Monitors["Monitors (Alerting)"]
        LogPatterns["Log Patterns (Anomaly Detection)"]
        LogMetrics["Log-Based Metrics"]
    end

    AppA --> DatadogAgent
    AppB --> DatadogAgent
    Infra --> DatadogAgent
    Infra --> Integrations

    DatadogAgent --> LogIngestion
    Integrations --> LogIngestion

    LogIngestion --> LogProcessing
    LogProcessing --> LogIndexing
    LogIndexing --> LogAnalytics

    LogIndexing --> LogExplorer
    LogAnalytics --> LogExplorer
    LogExplorer --> Dashboards
    LogExplorer --> Monitors
    LogAnalytics --> LogPatterns
    LogAnalytics --> LogMetrics

    Dashboards --> User["User"]
    Monitors --> User
    LogExplorer --> User
