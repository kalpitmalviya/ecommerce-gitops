# 🚀 eCommerce GitOps CI/CD Pipeline

> **Project Status:** ✅ Migration Complete (Jenkins → GitHub Actions)

![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)
![Kaniko](https://img.shields.io/badge/Kaniko-333333.svg?style=for-the-badge&logo=googlecloud&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-%23ef7b4d.svg?style=for-the-badge&logo=argo&logoColor=white)
![AWS EKS](https://img.shields.io/badge/AWS_EKS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)

## 📖 Overview

This repository hosts the source code and pipeline configurations for the eCommerce platform. We have modernized our delivery pipeline by moving from a legacy Jenkins workflow to a **Cloud-Native GitOps** approach.

The new pipeline utilizes **GitHub Actions** for Continuous Integration (CI) and **ArgoCD** for Continuous Deployment (CD), ensuring a fully declarative infrastructure state on **AWS EKS**.

---

## 🏗 Architecture & Workflow

The pipeline is triggered automatically on code commits. We utilize **Kaniko** for secure, dockerless image builds inside the CI runner, ensuring no privileged access is required.

```mermaid
graph TD
    User([Developer]) -->|git push| AppRepo[Application Repo]
    subgraph CI [Continuous Integration]
        AppRepo -->|Trigger| GHA[GitHub Actions]
        GHA -->|"Build (Rootless)"| Kaniko[Kaniko]
        Kaniko -->|Push Image| DH[DockerHub]
    end
    
    subgraph CD [GitOps Deployment]
        GHA -->|Update Tag| GitOpsRepo[GitOps Config Repo]
        GitOpsRepo -->|Detect Change| ArgoCD[ArgoCD Controller]
        ArgoCD -->|Sync State| EKS[AWS EKS Cluster]
    end

    style CI fill:#f0f6fc,stroke:#58a6ff,stroke-width:2px
    style CD fill:#f0f6fc,stroke:#fca326,stroke-width:2px
