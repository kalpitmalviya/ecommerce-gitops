## 🚀 CI/CD Pipeline Architecture

This project utilizes a modern, **GitOps-driven CI/CD pipeline**. We have successfully migrated from legacy Jenkins workflows to a cloud-native approach using **GitHub Actions** and **ArgoCD**. This ensures a strict separation between Integration (CI) and Deployment (CD) and enforces a fully declarative infrastructure.

### The Workflow

The pipeline is triggered automatically via Git events. We utilize **Kaniko** for secure, dockerless image building inside the CI runner, push artifacts to **DockerHub**, and trigger **ArgoCD** via a GitOps repository update to deploy changes to **AWS EKS**.

```mermaid
graph TD
    User([Developer]) -->|git push| AppRepo[Application Repo]
    subgraph CI [Continuous Integration - GitHub Actions]
        AppRepo -->|Trigger| GHA[GitHub Actions Runner]
        GHA -->|Build Image| Kaniko[Kaniko (Dockerless Build)]
        Kaniko -->|Push Artifact| DH[DockerHub Registry]
    end
    
    subgraph CD [Continuous Deployment - GitOps]
        GHA -->|Update Tag| GitOpsRepo[GitOps Config Repo]
        GitOpsRepo -->|Detect Change| ArgoCD[ArgoCD Controller]
        ArgoCD -->|Sync/Deploy| EKS[AWS EKS Cluster]
    end

    style CI fill:#f9f9f9,stroke:#333,stroke-width:2px,stroke-dasharray: 5 5
    style CD fill:#e6fffa,stroke:#333,stroke-width:2px
