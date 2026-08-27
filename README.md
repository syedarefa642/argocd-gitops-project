# 🚀 ArgoCD GitOps Deployment on Kubernetes

## 📌 Project Overview

This project demonstrates a **GitOps-based application deployment using ArgoCD and Kubernetes**.

The Kubernetes manifests are stored in **GitHub**, which acts as the **single source of truth**. ArgoCD continuously monitors the Git repository, compares the desired state defined in Git with the actual state of the Kubernetes cluster, and synchronizes the application when changes are detected.

The project deploys an **NGINX application** on a Kubernetes cluster running on an AWS EC2 instance.

---

## 🏗️ Architecture
<img width="1536" height="1024" alt="ChatGPT Image Aug 27, 2026, 08_13_08 PM" src="https://github.com/user-attachments/assets/3bf42f13-ac21-4c8d-8e4a-25d7648fd54c" />
**GitHub → ArgoCD → Kubernetes → NGINX**
### Workflow
1. Kubernetes manifests are stored in a GitHub repository.
2. ArgoCD monitors the GitHub repository.
3. ArgoCD compares the desired state in Git with the live state in Kubernetes.
4. When a difference is detected, ArgoCD marks the application as **OutOfSync**.
5. The changes can be synchronized manually or automatically.
6. ArgoCD applies the desired configuration to the Kubernetes cluster.
7. Kubernetes creates or updates the NGINX application.

---

## 🛠️ Technologies Used

* **AWS EC2** – Hosts the Kubernetes environment
* **Kubernetes** – Container orchestration
* **ArgoCD** – GitOps continuous delivery
* **GitHub** – Source code and configuration repository
* **NGINX** – Sample application
* **kubectl** – Kubernetes command-line tool
* **ArgoCD CLI** – ArgoCD management and deployment

---

## 📂 Project Structure

```text
argocd-gitops-project/
│
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
│
└── README.md
```

### Kubernetes Manifests

**deployment.yaml**

Defines the NGINX Deployment and manages the desired number of replicas.

**service.yaml**

Creates a NodePort Service to expose the NGINX application.

---

## ⚙️ Implementation

### 1. Kubernetes Cluster

A Kubernetes cluster was configured on an AWS EC2 instance.

The cluster was verified using:

```bash
kubectl get nodes
```

---

### 2. Install ArgoCD

Created the ArgoCD namespace:

```bash
kubectl create namespace argocd
```

Installed ArgoCD:

```bash
kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Verified the ArgoCD components:

```bash
kubectl get pods -n argocd
```

---

### 3. Configure GitHub Repository

The Kubernetes manifests were stored in the GitHub repository:

```text
argocd-gitops-project
```

GitHub acts as the **source of truth** for the Kubernetes desired state.

---

### 4. Create ArgoCD Application

The application was created using the ArgoCD CLI:

```bash
argocd app create nginx-gitops \
--repo https://github.com/syedarefa642/argocd-gitops-project.git \
--path k8s \
--dest-server https://kubernetes.default.svc \
--dest-namespace default
```

---
ArgoCD
<img width="1364" height="602" alt="Argo-cd" src="https://github.com/user-attachments/assets/4cff5802-19a0-4fe9-9de4-57c4e6c155ae" />

### 5. Synchronize the Application

The application was synchronized using:

```bash
argocd app sync nginx-gitops
```

Application status was verified using:

```bash
argocd app get nginx-gitops
```

The application reached:

```text
Sync Status:   Synced
Health Status: Healthy
```

---

## 🔄 GitOps & Drift Detection

To demonstrate GitOps, the NGINX Deployment was initially configured with:

```yaml
replicas: 2
```

The configuration was then changed in Git to:

```yaml
replicas: 3
```

After pushing the change to GitHub:

```bash
git add .
git commit -m "Scale nginx to 3 replicas"
git push
```

ArgoCD detected the difference between Git and the Kubernetes cluster and reported:

```text
Sync Status: OutOfSync
```

The change was then synchronized using:

```bash
argocd app sync nginx-gitops
```

After synchronization, Kubernetes was updated to run three NGINX replicas.

---

## 🔍 Verification

Check the Deployment:

```bash
kubectl get deployment nginx
```

Check the Pods:

```bash
kubectl get pods
```

Check the Service:

```bash
kubectl get svc
```

Check the ArgoCD application:

```bash
argocd app get nginx-gitops
```

Expected ArgoCD status:

```text
Sync Status:   Synced
Health Status: Healthy
```

---

## 🎯 Key Features Demonstrated

* ✅ GitOps workflow
* ✅ GitHub as the source of truth
* ✅ Kubernetes application deployment
* ✅ ArgoCD application management
* ✅ Manual synchronization
* ✅ Git-based deployment
* ✅ Configuration drift detection
* ✅ OutOfSync status detection
* ✅ Kubernetes Deployment and Service
* ✅ NGINX container deployment
* ✅ ArgoCD CLI usage

---

## 🧠 Key Learning

This project demonstrates how **ArgoCD implements GitOps for Kubernetes**.

Instead of directly making deployment changes in the Kubernetes cluster, the desired configuration is maintained in Git. ArgoCD continuously compares the Git configuration with the actual Kubernetes state and synchronizes the cluster with the desired state.

This provides better **visibility, consistency, traceability, and controlled deployments**.

---

## 🚀 Future Improvements

The project can be extended with:

* 🔹 ArgoCD Automatic Sync
* 🔹 Self-Healing
* 🔹 Application Rollback
* 🔹 Separate Dev and Production environments
* 🔹 Helm charts
* 🔹 GitHub Actions CI pipeline
* 🔹 Private GitHub repository authentication
* 🔹 Ingress and HTTPS
* 🔹 Monitoring with Prometheus and Grafana

---

## 👩‍💻 Author

**Syeda Refa**

DevOps / Cloud Enthusiast

GitHub: `https://github.com/syedarefa642`

