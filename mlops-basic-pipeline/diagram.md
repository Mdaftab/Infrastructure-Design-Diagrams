# Basic ML Training and Deployment Pipeline

```mermaid
graph LR
    subgraph "Data Preparation"
        DataIngestion["Data Ingestion"]
        DataPreprocessing["Data Preprocessing"]
    end

    subgraph "Model Development"
        ModelTraining["Model Training"]
        ModelEvaluation["Model Evaluation"]
    end

    subgraph "Model Deployment"
        ModelPackaging["Model Packaging"]
        ModelDeployment["Model Deployment"]
    end

    DataIngestion --> DataPreprocessing
    DataPreprocessing --> ModelTraining
    ModelTraining --> ModelEvaluation
    ModelEvaluation --> ModelPackaging
    ModelPackaging --> ModelDeployment
