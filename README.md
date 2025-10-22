<<<<<<< HEAD
# argocd-apps
Argocd Apps of App
=======
# ArgoCD Apps of Apps Example

This repository defines an ArgoCD "Apps of Apps" pattern managing multiple environments (dev, stag).

## Structure
- `apps_dev/` → Environment: **test-dev**
- `apps_stag/` → Environment: **test-stag**
- `apps-of-apps/` → Root ArgoCD Application

Each environment includes two apps: frontend & backend.

## Usage
1. Push this repo to GitHub.
2. Apply root application to ArgoCD:
   ```bash
   kubectl apply -f apps-of-apps/application.yaml -n argocd
   ```
3. ArgoCD will auto-sync and create all child Applications.
>>>>>>> 4d49384 (init argocd apps structure)
