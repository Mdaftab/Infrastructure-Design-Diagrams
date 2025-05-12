# Multi-Cloud Infrastructure with Terraform and Crossplane

This architecture demonstrates how to manage infrastructure across multiple cloud providers (GCP, AWS, Azure, vSphere) using a combination of Terraform and Crossplane, enabling infrastructure unification and multi-cloud provisioning from a single control plane.

## Use Case

An organization needs to manage infrastructure across multiple cloud environments and on-premises vSphere, aiming for a unified control plane and consistent provisioning workflows regardless of the underlying provider. They want to leverage existing Terraform expertise while adopting a Kubernetes-native approach for application teams.

## Architecture Overview

The system uses Terraform to provision foundational infrastructure in a management GCP project, including the management Kubernetes cluster. Crossplane is installed in this management cluster and configured with providers for GCP, AWS, Azure, and vSphere. Crossplane Compositions and XRDs (Composite Resource Definitions) define abstract infrastructure resources (like CompositeNetwork or CompositeCluster) that application teams can consume without needing to know the underlying cloud provider details.

## Key Components

### Terraform Control Plane
- **Terraform**: Used for provisioning the initial GCP infrastructure, including projects, networking, and the management GKE cluster.
- **Terraform State**: Stored remotely (e.g., in Cloud Storage or Terraform Cloud) for collaborative operations.
- **GCP Management Project**: Hosts the core infrastructure components managed by Terraform.

### Crossplane Control Plane
- **GKE Management Cluster**: A Kubernetes cluster running in the GCP management project, hosting the Crossplane installation.
- **Crossplane Controllers**: The core Crossplane components that reconcile desired state with actual state.
- **ProviderConfigs**: Kubernetes Custom Resources that configure Crossplane providers with credentials and settings for each cloud (GCP, AWS, Azure, vSphere).
- **Compositions & XRDs**: Define abstract interfaces for infrastructure resources, allowing users to provision complex, multi-provider infrastructure using simple Kubernetes manifests.

### Cloud Providers
- **Google Cloud Platform (GCP)**: Used for the management plane and potentially workload clusters.
- **Amazon Web Services (AWS)**: Target cloud for provisioning resources via Crossplane.
- **Microsoft Azure**: Target cloud for provisioning resources via Crossplane.
- **vSphere**: On-premises virtualization platform, target for provisioning resources via Crossplane.

### Workload Environments
- **GKE Dev/Stg/Prod Clusters**: Example workload clusters provisioned in GCP via Crossplane Compositions.
- **EKS DR Cluster (AWS)**: Example disaster recovery cluster provisioned in AWS via Crossplane.
- **AKS DR Cluster (Azure)**: Example disaster recovery cluster provisioned in Azure via Crossplane.
- **vSphere DR Cluster (On-Prem)**: Example disaster recovery cluster provisioned in vSphere via Crossplane.

### CI/CD Workflows
- **GitHub Actions**: Example CI/CD workflows for:
    - Provisioning the Terraform control plane (`infra.yml`).
    - Deploying applications to workload clusters (`app.yml`).
    - Provisioning disaster recovery infrastructure (`dr.yml`).

## Key Design Decisions

### Hybrid Infrastructure Management
- **Terraform for Foundation**: Using Terraform for initial setup and core management plane infrastructure leverages its strength in provisioning and managing cloud accounts and networks.
- **Crossplane for Workloads**: Using Crossplane for provisioning workload-specific infrastructure (clusters, databases, etc.) provides a Kubernetes-native API for application teams and enables infrastructure abstraction.

### Multi-Cloud Abstraction
- **Crossplane Compositions**: Defining abstract Composite Resources allows application teams to request infrastructure without needing deep knowledge of provider-specific APIs or configurations.
- **ProviderConfigs**: Centralized management of provider credentials and configurations within Crossplane.

### Disaster Recovery Strategy
- **Multi-Provider DR**: Demonstrating DR clusters across different providers (AWS, Azure, vSphere) showcases the flexibility of using Crossplane for diverse environments.
- **Automated DR Provisioning**: Including a dedicated CI/CD workflow for DR provisioning ensures that DR infrastructure can be reliably deployed.

### GitOps Integration
- **Declarative Infrastructure**: Both Terraform and Crossplane promote declarative configuration, aligning well with GitOps principles.
- **CI/CD Pipelines**: Automating infrastructure provisioning and application deployment through CI/CD workflows ensures consistency and traceability.

## Real-World Applications

This architecture is suitable for organizations that:

- Operate in a multi-cloud or hybrid cloud environment.
- Want to provide a self-service infrastructure platform to application teams via a Kubernetes API.
- Need to standardize infrastructure provisioning across different providers.
- Are looking to leverage both existing Terraform investments and adopt Kubernetes-native infrastructure management.
- Require automated provisioning of disaster recovery environments across different platforms.

The design provides a powerful pattern for unifying infrastructure management in complex, heterogeneous environments.
