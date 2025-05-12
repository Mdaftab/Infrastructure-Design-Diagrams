# Datadog Log Management Flow

```mermaid
graph LR
    subgraph "Applications & Infrastructure"
        AppA["Application A\n(Generates Logs)"]
        AppB["Application B\n(Generates Logs)"]
        Infra["Infrastructure\n(Hosts, Containers, Services)"]
    end

    subgraph "Datadog Agents & Integrations"
        DatadogAgent["Datadog Agent\n(Host/Container)"]
        Integrations["Cloud/Service Integrations\n(e.g., S3, CloudWatch Logs, Azure Blob Storage)"]
    end

    subgraph "Datadog Platform"
        LogIngestion["Log Ingestion\n(Receives Logs)"]
        LogProcessing["Log Processing Pipelines\n(Parsing, Enrichment, Filtering)"]
        LogIndexing["Log Indexing\n(Storage & Search)"]
        LogAnalytics["Log Analytics Engine"]
    end

    subgraph "Datadog UI & Features"
        LogExplorer["Log Explorer\n(Search & Filter)"]
        Dashboards["Dashboards\n(Visualization)"]
        Monitors["Monitors\n(Alerting)"]
        LogPatterns["Log Patterns\n(Anomaly Detection)"]
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
