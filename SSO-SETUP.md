# Verigen Applications Setup Guide

> **Note**: This file has been replaced by the comprehensive [DEPLOYMENT-GUIDE.md](./DEPLOYMENT-GUIDE.md) which covers all Verigen applications.

## Quick SSO Setup

### 1. Create Secrets
```bash
kubectl create secret generic sso-secrets \
  --from-literal=db-password=<secure-password> \
  --from-literal=jwt-secret=<secure-jwt-secret> \
  -n verigen-sso
```

### 2. Deploy Application
```bash
# ArgoCD automatically deploys from apps-of-apps
kubectl get applications -n argocd
```

### 3. Verify Deployment
```bash
kubectl get pods -n verigen-sso
kubectl get svc -n verigen-sso
kubectl get ingress -n verigen-sso
```

## SSO Components
1. **API** - Authentication service (port 8000)
2. **Dashboard** - Admin interface (port 3000)
3. **Worker** - Background processor
4. **PostgreSQL** - User database (port 5432)
5. **Redis** - Session cache (port 6379)
6. **Adminer** - DB management UI (port 8080)

## Domains
- **Production**: `sso-api.veriserve-vietnam.com`, `sso-admin.veriserve-vietnam.com`
- **Staging**: `sso-api-stg.stg.veriserve-vietnam.com`, `sso-admin-stg.stg.veriserve-vietnam.com`
- **Development**: `sso-api-dev.dev.veriserve-vietnam.com`, `sso-admin-dev.dev.veriserve-vietnam.com`

For complete deployment instructions, see [DEPLOYMENT-GUIDE.md](./DEPLOYMENT-GUIDE.md).
