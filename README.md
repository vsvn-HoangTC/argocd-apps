# ArgoCD Apps - Verigen Platform

ArgoCD configuration for deploying Verigen RAG and Chat applications using GitOps.

## Architecture

```
argocd-apps (this repo)
├── app-template/          # Shared Helm chart template
│   ├── templates/         # K8s resource templates
│   └── values.yaml        # Default values
└── apps-of-apps/          # ArgoCD Application manifests
    ├── application.yaml   # Root app-of-apps
    ├── verigen-rag-app.yaml
    └── verigen-chat-app.yaml

Application Repos (external):
├── vsvn-RAG/              # RAG app source + values
└── vsvn-verigenChat/      # Chat app source + values
```

## Multi-Source Pattern

Each application uses ArgoCD's multi-source feature:
- **Source 1**: Helm template from `argocd-apps/app-template`
- **Source 2**: Application-specific values from app repository

## Quick Start

### Deploy All Applications
```bash
kubectl apply -f apps-of-apps/application.yaml -n argocd
```

### Deploy Individual Applications
```bash
kubectl apply -f apps-of-apps/verigen-rag-app.yaml -n argocd
kubectl apply -f apps-of-apps/verigen-chat-app.yaml -n argocd
```

## Application Configuration

### Verigen RAG
- **Namespace**: `verigen-rag`
- **Values Repo**: https://github.com/vsvn-HoangTC/vsvn-RAG.git
- **Values Files**: `values.yaml`, `values-dev.yaml`

### Verigen Chat
- **Namespace**: `verigen-chat`
- **Values Repo**: https://github.com/vsvn-HoangTC/vsvn-verigenChat.git
- **Values Files**: `values.yaml`, `values-dev.yaml`

## Sync Policy

All applications use automated sync with:
- **Auto-prune**: Removes resources deleted from Git
- **Self-heal**: Reverts manual changes to match Git state
- **CreateNamespace**: Automatically creates target namespace

## Verify Deployment

```bash
# Check ArgoCD applications
kubectl get applications -n argocd

# Check deployed resources
kubectl get all -n verigen-rag
kubectl get all -n verigen-chat

# View application details
kubectl describe app verigen-rag -n argocd
kubectl describe app verigen-chat -n argocd
```

## Troubleshooting

### Application shows Synced but no resources
- Verify multi-source configuration in Application manifest
- Check that values files exist in application repository
- Review ArgoCD application controller logs

### Force resync
```bash
kubectl delete app <app-name> -n argocd
kubectl apply -f apps-of-apps/<app-name>.yaml
```
