# argo-gitops-enterprise
# 🏢 Enterprise Multi-Environment GitOps Architecture (`argo-gitops-enterprise`)

```mermaid
graph TD
    classDef developer fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px;
    classDef repo fill:#fff3e0,stroke:#ff9800,stroke-width:2px;
    classDef pipeline fill:#efebe9,stroke:#795548,stroke-width:2px;
    classDef argocd fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px;
    classDef cluster fill:#e8f5e9,stroke:#4caf50,stroke-width:2px;

    Dev[💻 DevOps Engineer]:::developer
    AppRepo[📦 Code Repo: argo-demo-app]:::repo
    GHA[🤖 GitHub Actions CI]:::pipeline
    GitOpsRepo[⚙️ GitOps Repo: argo-gitops-enterprise]:::repo
    ArgoCD[🐙 Argo CD Controller]:::argocd
    
    subgraph K8s [☸️ Kubernetes Target Cluster]
        Namespace[target-apps namespace]:::cluster
        DevPods[🧪 dev-python-web-app <br> 1 Replica]:::cluster
        ProdPods[🚀 prod-python-web-app <br> 3 Replicas]:::cluster
    end

    Dev -->|1. Git Push Code| AppRepo
    AppRepo -->|2. Triggers| GHA
    GHA -->|3. Updates Image Tag in envs/| GitOpsRepo
    GitOpsRepo -->|4. Polls Config Updates| ArgoCD
    ArgoCD -->|5. Reconciles State via Kustomize| Namespace
    Namespace --> DevPods
    Namespace --> ProdPods
```

This repository serves as the **Declarative Single Source of Truth** for a multi-environment Kubernetes infrastructure deployment. It utilizes **Kustomize** to manage variations across testing and production environments dynamically without code duplication.

## 📂 Repository Directory Breakdown

* **/base**: The structural foundation templates (`Deployment`, `Service`) shared globally by all runtime scopes.
* **/envs/dev**: Development overlay. Appends the `dev-` naming convention prefix and constraints cluster resource overhead to **1 active replica pod**.
* **/envs/prod**: Production high-availability overlay. Appends the `prod-` prefix and scales workloads to **3 distributed replica pods**.
* **/argocd-apps**: Declarative bootstrap manifests allowing Argo CD to natively mount and orchestrate both lifecycles cleanly.

## ⚙️ Declarative Deployment Setup

To ingest this infrastructure blueprint directly into an active Argo CD cluster:

```bash
# Apply both environment tracks simultaneously
kubectl apply -f argocd-apps/
```

## 🌟 Core AWS SAA-C03 Architectural Alignments Demonstrated
* **Decoupled Architecture:** Strict logical separation between continuous integration pipelines and continuous delivery states.
* **High Availability Configuration:** Dynamic vertical scale adjustments targeting and production environments dynamically without code duplication.








