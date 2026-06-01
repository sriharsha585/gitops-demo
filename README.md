# GitHub Actions + Argo CD GitOps Deployment

## Project Overview

This project demonstrates a complete GitOps-based Continuous Integration and Continuous Deployment (CI/CD) pipeline using GitHub Actions, Docker, Argo CD, and Kubernetes.

The pipeline automatically builds a Docker image, pushes it to Docker Hub, updates a GitOps repository, and deploys the latest version to Kubernetes using Argo CD.

---

## Architecture

```text
Developer
    │
    ▼
GitHub (Application Repository)
    │
    ▼
GitHub Actions
    │
    ├── Build Docker Image
    ├── Push Image to Docker Hub
    └── Update GitOps Repository
            │
            ▼
GitHub (GitOps Repository)
            │
            ▼
Argo CD
            │
            ▼
Kubernetes Cluster (Minikube)
```

---

## Technologies Used

* GitHub
* GitHub Actions
* Docker
* Docker Hub
* Kubernetes
* Minikube
* Argo CD
* GitOps

---

## Application Repository Structure

```text
sample-app/
│
├── Dockerfile
├── index.html
├── README.md
└── .github
    └── workflows
        └── deploy.yml
```

---

## GitOps Repository Structure

```text
gitops-demo/
│
├── deployment.yaml
├── service.yaml
└── README.md
```

---

## CI/CD Workflow

### Step 1: Developer Pushes Code

```bash
git add .
git commit -m "New feature"
git push origin main
```

---

### Step 2: GitHub Actions Triggered

Workflow is triggered automatically when code is pushed to the main branch.

```yaml
on:
  push:
    branches:
      - main
```

---

### Step 3: Checkout Source Code

```yaml
- uses: actions/checkout@v4
```

Downloads repository contents into the GitHub Actions runner.

---

### Step 4: Docker Hub Authentication

```yaml
- uses: docker/login-action@v3
```

Authenticates with Docker Hub using GitHub Secrets.

---

### Step 5: Build Docker Image

```bash
docker build -t nandam585/nginx:${{ github.run_number }} .
```

Builds a new Docker image.

---

### Step 6: Push Docker Image

```bash
docker push nandam585/nginx:${{ github.run_number }}
```

Pushes the image to Docker Hub.

---

### Step 7: Update GitOps Repository

The workflow:

1. Clones the GitOps repository
2. Updates deployment.yaml
3. Replaces the image tag with the latest version
4. Commits the change
5. Pushes the change back to GitHub

Example:

Before:

```yaml
image: nandam585/nginx:5
```

After:

```yaml
image: nandam585/nginx:6
```

---

### Step 8: Argo CD Synchronization

Argo CD continuously monitors the GitOps repository.

When it detects a change:

```text
OutOfSync
     ↓
Sync
     ↓
Healthy
```

The new version is automatically deployed to Kubernetes.

---

## GitHub Secrets Used

| Secret          | Purpose                            |
| --------------- | ---------------------------------- |
| DOCKER_USERNAME | Docker Hub Username                |
| DOCKER_PASSWORD | Docker Hub Password / Access Token |
| PAT_TOKEN       | GitHub Personal Access Token       |

---

## Kubernetes Deployment Verification

Check deployment:

```bash
kubectl get deployment
```

Check pods:

```bash
kubectl get pods
```

Check deployed image:

```bash
kubectl get deployment nginx \
-o=jsonpath='{.spec.template.spec.containers[0].image}'
```

---

## Argo CD Verification

List applications:

```bash
argocd app list
```

Application details:

```bash
argocd app get nginx-app
```

Manual sync:

```bash
argocd app sync nginx-app
```

---

## GitOps Concepts Learned

### GitOps

Git is the single source of truth.

### Desired State

Configuration stored in Git.

### Actual State

Running configuration in Kubernetes.

### Sync

Argo CD updates Kubernetes to match Git.

### Auto Sync

Automatically applies changes from Git.

### Self Heal

Automatically corrects configuration drift.

### Prune

Deletes Kubernetes resources removed from Git.

---

## Key Learning Outcomes

* GitHub Actions Workflow Creation
* GitHub Hosted Runners
* GitHub Secrets Management
* Docker Image Build & Push
* GitOps Repository Management
* Argo CD Continuous Deployment
* Kubernetes Deployments
* GitOps Best Practices
* CI/CD Troubleshooting

---

## Common Issues Faced

### Docker Authentication Failure

Cause:
Incorrect Docker Hub credentials.

Resolution:
Configured GitHub Secrets correctly.

---

### GitHub PAT Authentication Failure

Cause:
Insufficient token permissions.

Resolution:
Created Personal Access Token with Contents Read & Write permissions.

---

### Deployment Not Updating

Cause:
GitOps repository was not updated.

Resolution:
Verified image tag update and Git push operation.

---

### Argo CD OutOfSync

Cause:
Git repository and Kubernetes state differed.

Resolution:
Performed synchronization and enabled Auto Sync.

---

## Future Enhancements

* Helm Integration
* Kustomize Integration
* Multi-Environment Deployment
* Prometheus Monitoring
* Grafana Dashboards
* RBAC
* SSO Integration
* High Availability Argo CD
* Disaster Recovery Strategy

---

## Author

Sai Sri Harsha Nandam

DevOps Engineer

Skills:
AWS | GitHub Actions | Jenkins | Docker | Kubernetes | Argo CD | Terraform | Ansible | Linux | GitOps
