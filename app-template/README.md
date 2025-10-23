# ArgoCD Application Template

Common template for creating ArgoCD applications for all apps.

## Usage

### 1. Create new app using Helm
```bash
helm template my-app ./app-template -f ./app-template/values-my-app.yaml > argocd-applications/my-app.yaml
```

### 2. Example: Create verigen-chat app
```bash
helm template verigen-chat ./app-template -f ./app-template/values-verigen-chat.yaml > argocd-applications/verigen-chat-new.yaml
```

### 3. Example: Create RAG app
```bash
helm template rag-app ./app-template -f ./app-template/values-rag.yaml > argocd-applications/rag-new.yaml
```

## Values File Structure

Create `values-[app-name].yaml` file with structure:

```yaml
appName: "app-name"
argoNamespace: "argocd"

source:
  repoURL: "https://github.com/your-org/argocd-apps"
  path: "apps-[app-folder]"
  helm:
    valueFiles:
      - values.yaml
      - values-dev.yaml

destination:
  namespace: "target-namespace"
```