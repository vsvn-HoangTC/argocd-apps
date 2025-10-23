# ArgoCD Apps (RAG + VerigenChat)

This repository deploys the **RAG** and **VerigenChat** systems using ArgoCD + Helm.

## Environments
| Environment | Domain | Helm Values |
|--------------|-----------------------------|-------------------|
| dev | *.dev.veriserve-vietnam.com | values-dev.yaml |
| stag | *.stag.veriserve-vietnam.com | values-stag.yaml |

## Deploy Flow

```bash
kubectl apply -f apps-of-apps/application.yaml -n argocd
