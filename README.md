# argo-gitops-enterprise
Kustomize k8s demo

argo-gitops-enterprise/
├── base/
│   ├── deployment.yaml
│   ├── kustomization.yaml
│   └── service.yaml
└── envs/
    ├── dev/
    │   └── kustomization.yaml
    └── prod/
        └── kustomization.yaml


graph TD
    %% Define Styles
    classDef developer fill:#e1f5fe,stroke:#03a9f4,stroke-width:2px;
    classDef repo fill:#fff3e0,stroke:#ff9800,stroke-width:2px;
    classDef pipeline fill:#efebe9,stroke:#795548,stroke-width:2px;
    classDef argocd fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px;
    classDef cluster fill:#e8f5e9,stroke:#4caf50,stroke-width:2px;

    %% Elements
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

    %% Flows
    Dev -->|1. Git Push Code| AppRepo
    AppRepo -->|2. Triggers| GHA
    GHA -->|3. Updates Image Tag in envs/| GitOpsRepo
    GitOpsRepo -->|4. Polls Config Updates| ArgoCD
    ArgoCD -->|5. Reconciles State via Kustomize| Namespace
    Namespace --> DevPods
    Namespace --> ProdPods










