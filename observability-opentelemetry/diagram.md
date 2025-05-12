# OpenTelemetry Observability Architecture

```mermaid
graph LR
    subgraph "Applications & Services"
        AppA["Application A\n(Instrumented)"]
        AppB["Application B\n(Instrumented)"]
        Infra["Infrastructure\n(Hosts, Containers)"]
    end

    subgraph "OpenTelemetry Components"
        OT_SDK["OTel SDKs\n(in Applications)"]
        OT_Collector["OTel Collector\n(Agent/Gateway)"]
    end

    subgraph "Backend Systems"
        MetricsBackend["Metrics Backend\n(e.g., Prometheus, Cloud Monitoring)"]
        TracesBackend["Traces Backend\n(e.g., Jaeger, Tempo, Cloud Trace)"]
        LogsBackend["Logs Backend\n(e.g., Loki, OpenSearch, Cloud Logging)"]
        AnalyticsBackend["Analytics Backend\n(e.g., BigQuery, Snowflake)"]
    end

    subgraph "Analysis & Visualization"
        Dashboarding["Dashboarding\n(e.g., Grafana, Looker Studio)"]
        Alerting["Alerting System"]
        AnalyticsTools["Analytics Tools"]
    end

    AppA --> OT_SDK
    AppB --> OT_SDK
    Infra --> OT_Collector

    OT_SDK --> OT_Collector

    OT_Collector --> MetricsBackend
    OT_Collector --> TracesBackend
    OT_Collector --> LogsBackend
    OT_Collector --> AnalyticsBackend

    MetricsBackend --> Dashboarding
    TracesBackend --> Dashboarding
    LogsBackend --> Dashboarding
    MetricsBackend --> Alerting
    LogsBackend --> Alerting

    AnalyticsBackend --> AnalyticsTools
    Dashboarding --> Alerting
