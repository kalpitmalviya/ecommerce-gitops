git push (app repo)
        ↓
GitHub Actions
        ↓
Kaniko builds image
        ↓
Push to DockerHub
        ↓
GitOps repo updated
        ↓
ArgoCD detects change
        ↓
Deploys to EKS
