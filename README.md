# 🚀 Multi-Environment DevOps Deployment

This project demonstrates a **production-style DevOps pipeline** where a full-stack application (frontend + backend) is containerized, deployed to Kubernetes (AWS EKS), and managed across **multiple environments (Dev, Staging, Prod)** using **Helm and Jenkins CI/CD**.

---

# 📌 Project Overview

This project implements:

* 🐳 Dockerized frontend & backend applications
* ☸️ Kubernetes deployment on AWS EKS
* 📦 Helm for templated multi-environment deployments
* 🔁 Jenkins CI/CD pipeline for automated builds & deployments
* 🌐 NGINX Ingress Controller for routing
* ⚖️ Path-based routing with rewrite rules
* ☁️ AWS LoadBalancer (ELB) for external access

---

# 🏗️ Architecture

```
User (Browser)
      ↓
AWS ELB (LoadBalancer)
      ↓
NGINX Ingress Controller
      ↓
Path-based Routing + Rewrite
      ↓
-------------------------------------
|     /dev       → Dev Environment   |
|     /staging   → Staging Env       |
|     /          → Production Env    |
-------------------------------------
      ↓
Kubernetes Services
      ↓
Pods (Frontend + Backend)
```

---

# 🌐 Environments & Access

| Environment | URL                        |
| ----------- | -------------------------- |
| 🟢 Dev      | `http://<ELB-DNS>/dev`     |
| 🟡 Staging  | `http://<ELB-DNS>/staging` |
| 🔵 Prod     | `http://<ELB-DNS>/`        |

---

# ⚙️ Tech Stack

### 🚀 DevOps Tools

* AWS EKS (Kubernetes)
* Helm (Templating & deployments)
* Jenkins (CI/CD pipeline)
* Docker (Containerization)

### 🌐 Networking

* NGINX Ingress Controller
* AWS ELB (LoadBalancer)

### 💻 Application

* Frontend: React / Vite
* Backend: Node.js / Express (or your backend stack)

---

# 📂 Project Structure

```
cloudblitz-app/
│
├── frontend/              # UI application
├── backend/               # API application
│
├── my-app/                # Helm chart
│   ├── templates/
│   ├── values.yaml
│
├── env/                   # Environment-specific configs
│   ├── dev-values.yaml
│   ├── staging-values.yaml
│   ├── prod-values.yaml
│
├── Jenkinsfile            # CI/CD pipeline
├── README.md
├── .gitignore
```

---

# 🔁 CI/CD Pipeline Flow

1. Code pushed to GitHub
2. Jenkins pipeline triggered
3. Docker images built for frontend & backend
4. Images pushed to Docker Hub
5. Helm deploys updated images to EKS
6. Ingress routes traffic to correct environment

---

# 📦 Helm Deployment

Helm is used to manage deployments across environments:

### Dev

```bash
helm upgrade --install myapp ./my-app -f env/dev-values.yaml -n dev
```

### Staging

```bash
helm upgrade --install myapp ./my-app -f env/staging-values.yaml -n staging
```

### Production

```bash
helm upgrade --install myapp ./my-app -f env/prod-values.yaml -n prod
```

---

# 🌐 Ingress Configuration

* Uses **NGINX Ingress Controller**
* Implements **path-based routing**
* Uses **rewrite rules** to map paths correctly

### Example:

| Path       | Routes To           |
| ---------- | ------------------- |
| `/dev`     | Dev frontend        |
| `/dev/api` | Dev backend         |
| `/staging` | Staging frontend    |
| `/`        | Production frontend |

---

# 🔐 Security Considerations

* Sensitive data (AWS keys, `.env`, kubeconfig) are excluded using `.gitignore`
* Jenkins credentials are securely stored
* No secrets are exposed in the repository

---

# 🚀 Key Features

* ✅ Multi-environment deployment (Dev, Staging, Prod)
* ✅ Automated CI/CD pipeline using Jenkins
* ✅ Containerized microservices architecture
* ✅ Kubernetes-native deployment with Helm
* ✅ Ingress-based routing with rewrite rules
* ✅ Scalable and production-ready setup

---

# 🧠 What I Learned

* Designing multi-environment Kubernetes deployments
* Using Helm for dynamic configuration management
* Implementing CI/CD pipelines with Jenkins
* Debugging Ingress and networking issues
* Managing path-based routing and URL rewrites
* Handling real-world DevOps challenges

---

# 🚀 Future Enhancements

* 🔐 Add HTTPS using cert-manager
* 🌐 Configure custom domain using AWS Route53
* 📊 Add monitoring (Prometheus + Grafana)
* 🔍 Centralized logging (ELK stack)
* ⚙️ Infrastructure as Code (Terraform)

---

# 👩‍💻 Author

**Sanika Kate**
DevOps & Cloud Enthusiast

---

# ⭐ Final Note

This project demonstrates a **complete DevOps lifecycle** — from development to deployment — using modern tools and best practices.

👉 It reflects real-world production architecture and hands-on DevOps expertise.

