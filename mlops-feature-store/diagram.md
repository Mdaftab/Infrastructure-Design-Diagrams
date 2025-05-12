# MLOps with Feature Store

```mermaid
graph LR
    subgraph "Data Sources"
        SourceA["Source A"]
        SourceB["Source B"]
    end

    subgraph "Feature Engineering"
        FeatureProcessing["Feature Processing\n(Batch/Streaming)"]
    end

    subgraph "Feature Store"
        FeatureStore["Feature Store\n(Online/Offline)"]
    end

    subgraph "ML Pipelines"
        TrainingPipeline["Training Pipeline"]
        InferencePipeline["Inference Pipeline"]
    end

    subgraph "Model Registry"
        ModelRegistry["Model Registry"]
    end

    subgraph "Serving Layer"
        OnlineServing["Online Serving\n(e.g., API Endpoint)"]
        BatchServing["Batch Serving"]
    end

    SourceA --> FeatureProcessing
    SourceB --> FeatureProcessing

    FeatureProcessing --> FeatureStore

    FeatureStore --> TrainingPipeline
    FeatureStore --> InferencePipeline

    TrainingPipeline --> ModelRegistry
    ModelRegistry --> InferencePipeline

    InferencePipeline --> OnlineServing
    InferencePipeline --> BatchServing

    OnlineServing --> User["End User"]
    BatchServing --> Analytics["Analytics/Reporting"]
