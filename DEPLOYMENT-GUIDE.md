# Verigen Applications Deployment Guide

This guide provides step-by-step instructions for deploying all Verigen applications using ArgoCD and Helm.

## Prerequisites

- Kubernetes cluster with ArgoCD installed
- kubectl configured to access the cluster
- Docker registry access (dockerhub-secret configured)
- Domain names configured for ingress

## Available Applications

### 1. SSO AllAItech (`apps-sso-allAItech`)
- **API Service**: Authentication backend (port 8000)
- **Dashboard**: Admin interface (port 3000)
- **Worker**: Background job processor
- **PostgreSQL**: User data storage
- **Redis**: Session and cache storage
- **Adminer**: Database management UI (port 8080)

### 2. RAG Application (`apps-RAG`)
- **Frontend**: Web interface (port 80)
- **Backend**: API service (port 8000)
- **Worker**: Document processing
- **PostgreSQL**: Application data
- **MongoDB**: Document storage
- **Redis**: Cache layer
- **Qdrant**: Vector database

### 3. Verigen Chat (`apps-verigenChat`)
- Chat application components
- Real-time messaging services
- Database and cache layers

## Deployment Steps

### Step 1: Prepare Secrets

Create required secrets for each application:

```bash
# SSO Application Secrets
kubectl create secret generic sso-secrets \
  --from-literal=db-password=<secure-db-password> \
  --from-literal=jwt-secret=<secure-jwt-secret> \
  -n verigen-sso

# RAG Application Secrets
kubectl create secret generic rag-secrets \
  --from-literal=jwt-secret=<secure-jwt-secret> \
  --from-literal=openai-key=<openai-api-key> \
  -n verigen-rag

# Chat Application Secrets
kubectl create secret generic chat-secrets \
  --from-literal=db-password=<secure-db-password> \
  --from-literal=jwt-secret=<secure-jwt-secret> \
  -n verigen-chat
```

### Step 2: Configure Domain Names

Update DNS records to point to your Kubernetes ingress:

#### Production Domains
- SSO: `sso-api.veriserve-vietnam.com`, `sso-admin.veriserve-vietnam.com`
- RAG: `verigen-rag.veriserve-vietnam.com`
- Chat: `verigen-chat.veriserve-vietnam.com`

#### Development Domains
- SSO: `sso-api-dev.dev.veriserve-vietnam.com`, `sso-admin-dev.dev.veriserve-vietnam.com`
- RAG: `verigen-rag-dev.dev.veriserve-vietnam.com`
- Chat: `verigen-chat-dev.dev.veriserve-vietnam.com`

#### Staging Domains
- SSO: `sso-api-stg.stg.veriserve-vietnam.com`, `sso-admin-stg.stg.veriserve-vietnam.com`
- RAG: `verigen-rag-stg.stg.veriserve-vietnam.com`
- Chat: `verigen-chat-stg.stg.veriserve-vietnam.com`

### Step 3: Deploy Applications

Applications are automatically deployed when ArgoCD detects changes in the apps-of-apps repository:

```bash
# Check ArgoCD applications
kubectl get applications -n argocd

# Monitor deployment status
kubectl get pods -n verigen-sso
kubectl get pods -n verigen-rag
kubectl get pods -n verigen-chat
```

### Step 4: Verify Deployments

Check services and ingress:

```bash
# Check services
kubectl get svc -n verigen-sso
kubectl get svc -n verigen-rag
kubectl get svc -n verigen-chat

# Check ingress
kubectl get ingress -n verigen-sso
kubectl get ingress -n verigen-rag
kubectl get ingress -n verigen-chat
```

## Environment Management

### Development Environment
- Uses `values-dev.yaml` files
- Lower resource limits
- Development image tags
- `.dev.veriserve-vietnam.com` domains

### Staging Environment
- Uses `values-stg.yaml` files
- Production-like resources
- Staging image tags
- `.stg.veriserve-vietnam.com` domains
- TLS certificates enabled

### Production Environment
- Uses `values.yaml` files
- Full resource allocation
- Latest stable image tags
- `.veriserve-vietnam.com` domains
- TLS certificates and security hardening

## Troubleshooting

### Common Issues

1. **Pod not starting**
   ```bash
   kubectl describe pod <pod-name> -n <namespace>
   kubectl logs <pod-name> -n <namespace>
   ```

2. **Service not accessible**
   ```bash
   kubectl get endpoints -n <namespace>
   kubectl describe svc <service-name> -n <namespace>
   ```

3. **Ingress not working**
   ```bash
   kubectl describe ingress -n <namespace>
   # Check DNS resolution
   nslookup <domain-name>
   ```

4. **Database connection issues**
   ```bash
   kubectl exec -it <pod-name> -n <namespace> -- env | grep DB
   kubectl logs <db-pod-name> -n <namespace>
   ```

### ArgoCD Management

```bash
# Sync application manually
argocd app sync <app-name>

# Check application health
argocd app get <app-name>

# View application logs
argocd app logs <app-name>
```

## Security Considerations

1. **Update default passwords** in production values files
2. **Enable TLS** for all production ingress
3. **Use strong JWT secrets** for authentication
4. **Regularly update** container images
5. **Monitor** application logs for security events
6. **Backup** databases regularly

## Monitoring and Maintenance

- Monitor resource usage and scale as needed
- Update image tags for new releases
- Review and rotate secrets periodically
- Monitor application health through ArgoCD dashboard
- Set up alerts for critical services

## Support

For issues and questions:
- Check ArgoCD dashboard for application status
- Review Kubernetes events and logs
- Consult individual application README files
- Contact the development team for application-specific issues