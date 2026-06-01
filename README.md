# Jenkins + Argo CD GitOps Demo

## Project Overview

This project demonstrates a complete GitOps-based CI/CD pipeline using:

- Jenkins
- Docker
- Docker Hub
- GitHub
- Argo CD
- Kubernetes (Minikube)

The goal is to automate application deployment using GitOps principles, where Argo CD continuously synchronizes Kubernetes with the desired state stored in Git.

---

## Architecture

```text
Developer
    |
    v
GitHub (Application Repository)
    |
    v
Jenkins Pipeline
    |
    +--> Build Docker Image
    |
    +--> Push Image to Docker Hub
    |
    +--> Update GitOps Repository
    |
    v
GitHub (GitOps Repository)
    |
    v
Argo CD
    |
    v
Kubernetes (Minikube)
```

---

## Technologies Used

| Tool | Purpose |
|--------|----------|
| GitHub | Source Code Management |
| Jenkins | Continuous Integration |
| Docker | Containerization |
| Docker Hub | Image Registry |
| Argo CD | GitOps Deployment Tool |
| Kubernetes | Container Orchestration |
| Minikube | Local Kubernetes Cluster |

---

## Repository Structure

### Application Repository

```text
sample-app/
│
├── Dockerfile
├── Jenkinsfile
├── index.html
└── README.md
```

### GitOps Repository

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

Developer commits code changes to GitHub.

```bash
git add .
git commit -m "Code changes"
git push origin main
```

---

### Step 2: Jenkins Pipeline Starts

Jenkins automatically:

- Checks out source code
- Builds Docker image
- Tags image using build number

Example:

```bash
docker build -t nandam585/nginx:15 .
```

---

### Step 3: Push Image to Docker Hub

Jenkins authenticates with Docker Hub and pushes the image.

```bash
docker push nandam585/nginx:15
```

---

### Step 4: Update GitOps Repository

Jenkins updates deployment.yaml with the new image tag.

Before:

```yaml
image: nandam585/nginx:14
```

After:

```yaml
image: nandam585/nginx:15
```

Jenkins commits and pushes the change.

```bash
git commit -m "Updated image to 15"
git push
```

---

### Step 5: Argo CD Detects Change

Argo CD continuously watches the GitOps repository.

When a new commit is detected:

```text
OutOfSync
     ↓
Sync
     ↓
Healthy
```

---

### Step 6: Kubernetes Deployment Updated

New pods are deployed automatically.

```bash
kubectl get pods
```

---

## Jenkins Pipeline Stages

### Debug

```groovy
pwd
ls -la
```

Verifies workspace contents.

---

### Build

```groovy
docker build -t $IMAGE:$BUILD_NUMBER .
```

Builds Docker image.

---

### Docker Login & Push

```groovy
docker login
docker push
```

Pushes image to Docker Hub.

---

### Update GitOps Repo

Updates Kubernetes manifests with latest image version.

---

## Argo CD Concepts Learned

### GitOps

Git acts as the single source of truth.

---

### Desired State

Configuration stored in Git.

Example:

```yaml
replicas: 2
```

---

### Actual State

Current state inside Kubernetes cluster.

---

### OutOfSync

Git and Kubernetes states do not match.

---

### Sync

Argo CD updates Kubernetes to match Git.

---

## Issues Faced and Resolutions

### Issue 1

#### Error

```text
docker: not found
```

#### Root Cause

Docker CLI missing inside Jenkins container.

#### Resolution

Installed Docker CLI.

---

### Issue 2

#### Error

```text
permission denied while trying to connect to docker daemon
```

#### Root Cause

Jenkins user lacked Docker socket permissions.

#### Resolution

Added Jenkins user to Docker socket group.

---

### Issue 3

#### Error

```text
Dockerfile not found
```

#### Root Cause

Missing Dockerfile in repository.

#### Resolution

Created Dockerfile and pushed to GitHub.

---

### Issue 4

#### Error

```text
COPY index.html failed
```

#### Root Cause

File missing from build context.

#### Resolution

Added index.html to repository.

---

### Issue 5

#### Error

```text
docker push access denied
```

#### Root Cause

Docker Hub authentication not configured.

#### Resolution

Configured Docker Hub credentials in Jenkins.

---

### Issue 6

#### Error

```text
Credential type mismatch
```

#### Root Cause

Secret Text and Username/Password mismatch.

#### Resolution

Updated Jenkins credential configuration.

---

### Issue 7

#### Error

```text
git push failed
```

#### Root Cause

GitHub PAT authentication issue.

#### Resolution

Configured GitHub credentials correctly.

---

## Key Learnings

- GitOps Fundamentals
- Jenkins Pipeline Development
- Docker Image Management
- Docker Hub Integration
- GitHub Authentication
- Argo CD Deployment Workflow
- Kubernetes Application Deployment
- CI/CD Troubleshooting
- Production Support Scenarios

---

## Future Enhancements

- Auto Sync
- Self Heal
- Prune
- Helm Integration
- Multi-Environment Deployments
- RBAC
- Monitoring with Prometheus & Grafana
- High Availability Setup

---

## Author

Sai Sri Harsha Nandam

DevOps Engineer

Technologies:
AWS | Jenkins | Docker | Kubernetes | Argo CD | GitHub Actions | Terraform | Ansible | Linux
