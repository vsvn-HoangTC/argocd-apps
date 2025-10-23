# ArgoCD Apps (RAG + VerigenChat)

This repository deploys the **RAG** and **VerigenChat** systems using ArgoCD + Helm.

## Repository Structure
- `apps-RAG/`: Helm chart for RAG application
- `apps-verigenChat/`: Helm chart for VerigenChat application  
- `argocd-apps/`: ArgoCD apps-of-apps pattern configuration

## Environments
| Environment | Domain | Helm Values | Namespace |
|-------------|--------|-------------|----------|
| dev | *.dev.veriserve-vietnam.com | values-dev.yaml | rag-system, verigen-chat |
| stag | *.stag.veriserve-vietnam.com | values-stag.yaml | rag-system, verigen-chat |

## Deploy Options

### Option 1: Apps-of-Apps Pattern (Recommended)
```bash
kubectl apply -f apps-of-apps/application.yaml -n argocd
```

### Option 2: Individual Applications
```bash
kubectl apply -f apps-of-apps/rag-app.yaml -n argocd
kubectl apply -f apps-of-apps/verigen-chat-app.yaml -n argocd
