# ArgoCD kustomization

## Installation

### Prerequisities
* Existing *argocd-system* namespace

### Install ArgoCD manifest
```bash
kubectl create -f build/argocd_<VERSION>.yaml -n argocd-system
```

### Configuration
* Change default password for admin user
```bash
ARGO_ADMIN_PASS=<PASSWORD>
ARGO_ADMIN_PASS_ENCODED=$(htpasswd -bnBC 10 '' $ARGO_ADMIN_PASS | tr -d ':\n')

kubectl -n argocd-system patch secret argocd-secret \
    -p '{"stringData": {
        "admin.password": "'$ARGO_ADMIN_PASS_ENCODED'",
        "admin.passwordMtime": "$(date +%FT%T%Z)"
    }}'
```

## Build a new version

### Prerequisities
* Kustomize (version >= 3.1.0)

### Build
```bash
kustomize build base > build/argocd_<VERSION>.yaml
```