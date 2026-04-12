# 🚀 GitOps Deployment using Argo CD and Kubernetes

## 📌 Project Overview

This project demonstrates a **GitOps-based deployment pipeline** using **Argo CD** and **Kubernetes**, where application deployment is fully managed through a Git repository.

Instead of manually applying Kubernetes manifests, **Argo CD continuously monitors GitHub** and ensures the cluster state always matches the desired state defined in Git.

---

## 🧠 What is GitOps?

GitOps is a modern DevOps practice where:

* **Git is the single source of truth**
* All infrastructure and application configurations are stored in Git
* Changes are automatically applied to the system via tools like Argo CD

---

## 🏗️ Architecture

```
Developer → GitHub Repo → Argo CD → Kubernetes Cluster
```

### Workflow:

1. Developer pushes code/manifests to GitHub
2. Argo CD detects changes in the repository
3. Argo CD syncs the changes to Kubernetes
4. Kubernetes updates the application automatically

---

## 🛠️ Tech Stack

* **Kubernetes (kind)** – Local cluster
* **Argo CD** – GitOps continuous delivery tool
* **Docker** – Containerization
* **GitHub** – Source of truth
* **kubectl** – Kubernetes CLI

---

## 📂 Project Structure

```
gitops-argocd-demo/
├── app/
│   ├── namespace.yaml
│   ├── deployment.yaml
│   └── service.yaml
├── argocd/
│   └── application.yaml
├── screenshots/
└── README.md
```

---

## ⚙️ Prerequisites

Make sure you have installed:

* Docker
* kubectl
* kind
* Git
* Argo CD CLI

---

## 🚀 Setup Instructions

### 1️⃣ Create Kubernetes Cluster

```bash
kind create cluster --name argocd-demo
kubectl get nodes
```

---

### 2️⃣ Install Argo CD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Check pods:

```bash
kubectl get pods -n argocd
```

---

### 3️⃣ Access Argo CD UI

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open browser:

```
https://localhost:8080
```

---

### 4️⃣ Get Admin Password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret \
-o jsonpath="{.data.password}" | base64 -d && echo
```

Login:

* Username: `admin`
* Password: (from above command)

---

### 5️⃣ Push Code to GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/gitops-argocd-demo.git
git push -u origin main
```

---

### 6️⃣ Deploy Application via Argo CD

```bash
kubectl apply -f argocd/application.yaml
```

Verify:

```bash
kubectl get applications -n argocd
```

---

## 🔄 GitOps in Action

### ✅ Auto Sync

* Update `deployment.yaml` (change image version)
* Push changes to GitHub
* Argo CD automatically updates the cluster

---

### ♻️ Self-Healing

Break the cluster manually:

```bash
kubectl scale deployment demo-app -n demo-app --replicas=1
```

Argo CD will automatically restore it back to desired state (`replicas=2`).

---

### 🔙 Rollback

#### Option 1: Git Revert

```bash
git revert <commit_id>
git push
```

#### Option 2: Argo CD CLI

```bash
argocd app history demo-app
argocd app rollback demo-app
```

---

## 📊 Key Features Implemented

* ✅ Git as Single Source of Truth
* ✅ Declarative Kubernetes Manifests
* ✅ Automated Sync (Continuous Deployment)
* ✅ Self-Healing (Drift Correction)
* ✅ Rollback Capability
* ✅ Namespace Auto-Creation

---

## 📸 Screenshots (Add here)

* Argo CD Dashboard
* Synced Application
* Self-Healing Demo
* Rollback History

---

## 🧪 Useful Commands

```bash
kubectl get all -n argocd
kubectl get all -n demo-app
kubectl describe application demo-app -n argocd
argocd app get demo-app
argocd app history demo-app
```

---

## ❗ Common Issues & Fixes

### Port already in use

```bash
sudo lsof -i :8080
kill -9 <PID>
```

---

### Application OutOfSync

* Check GitHub repo URL
* Verify branch (`main`)
* Check path (`app/`)
* Ensure manifests are pushed

---

### Namespace not found

Make sure:

```yaml
CreateNamespace=true
```

---

## 🎯 Resume Description

**GitOps Deployment using Argo CD and Kubernetes**

* Implemented GitOps-based deployment pipeline using Argo CD
* Managed Kubernetes applications using declarative manifests stored in GitHub
* Enabled automated sync, self-healing, and rollback mechanisms
* Built and tested complete system locally using kind cluster
* Demonstrated real-time deployment updates via Git commits

---

## 🚀 Future Improvements

* Helm integration
* Kustomize overlays (dev/prod)
* CI pipeline to update image tags
* Argo CD ApplicationSets
* Multi-cluster deployment

---

## 👨‍💻 Author

**Arshit Choubey**
DevOps Engineer

---

## ⭐ If you like this project

Give it a star ⭐ on GitHub and connect with me!
