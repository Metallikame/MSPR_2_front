# MSPR_2_front — Frontend COFRAP

Interface web du portail d'authentification sécurisé COFRAP.

## Structure

```
MSPR_2_front/
├── src/
│   └── index.html              # Frontend (HTML/CSS/JS standalone)
├── k8s/
│   └── deployment.yaml         # Deployment + Service Kubernetes
├── argocd/
│   └── application.yaml        # Application ArgoCD à appliquer 1 fois
├── .github/
│   └── workflows/ci.yaml       # Build Docker → GHCR → update k8s tag
├── Dockerfile                  # nginx:alpine + proxy /function/ → OpenFaaS
└── nginx.conf                  # Config nginx avec reverse proxy backend
```

## Déploiement

### 1. Enregistrer l'Application dans ArgoCD

```bash
kubectl apply -f argocd/application.yaml
```

Ou via l'UI ArgoCD → **NEW APP** avec :
- **Repository URL** : `https://github.com/Metallikame/MSPR_2_front`
- **Path** : `k8s`
- **Namespace** : `cofrap`
- **Branch** : `main`

### 2. Pusher le code → CI/CD automatique

```bash
git add .
git commit -m "feat: initial frontend"
git push origin main
```

GitHub Actions construit l'image → push vers `ghcr.io/metallikame/mspr_2_front:latest` → met à jour `k8s/deployment.yaml` → ArgoCD synchronise automatiquement.

## Accès

```
http://<NODE_IP>:30080
```
