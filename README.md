# 🚀 Swiggi Clone – Kubernetes Deployment with HPA & Monitoring

This project demonstrates a full Kubernetes deployment of a Swiggi-clone application using:

- AWS EC2
- Kind Cluster
- Docker Hub Image
- Kubernetes Deployment, Service, Namespace, HPA
- Prometheus + Grafana Monitoring
- Metrics Server
- Fake Traffic Load Testing for HPA

**A perfect DevOps portfolio project showcasing real-world infrastructure and scaling concepts.**

---

## 🏗️ Project Architecture Overview

### ✔ AWS EC2 Instance
- Ubuntu EC2 used as Kubernetes host
- Security Group ports opened:
  - `3000, 30001, 30002, 30003, 30004, 30005`

### ✔ Docker Image
Used application image from Docker Hub:
```bash
docker pull daxdobariya/swiggi_clone
```
- App runs on port `3000`

### ✔ Kind Cluster Setup
Kind cluster configured with port mappings:
- `3000 → 3000`
- `30001–30005 → 30001–30005`

This allows NodePort services to be accessible from outside the EC2 instance.

---

## 📦 Kubernetes Components Used

### 🏷 Namespace
Project-specific namespace:
```
swiggi-clone
```

### 🚀 Deployment
- **Image:** `daxdobariya/swiggi_clone`
- **Container port:** `3000`
- **Replica count:** `2`

### 🌐 NodePort Service
Exposes the application on:
- **NodePort:** `30001`

### 📈 Horizontal Pod Autoscaler (HPA)
Automatically scales pods based on CPU usage.
- **Min replicas:** `2`
- **Max replicas:** `5`
- **CPU threshold:** `60%`

---

## 📊 Monitoring Setup

### ✔ Prometheus + Grafana (Using Helm)
Installed using:
```bash
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
```

### ✔ NodePort Services (patched)
- **Grafana** → NodePort: `30002`
- **Prometheus** → NodePort: `30003`

### ✔ Metrics Server
Installed and patched for Kind cluster to enable:
```bash
kubectl top pod
kubectl top node
```
- HPA functionality

---

## 🔥 HPA Load Testing (Fake Traffic)

To test HPA scaling, fake traffic was generated using PowerShell:

```powershell
while ($true) {
    1..200 | ForEach-Object { Invoke-WebRequest -Uri "http://<EC2-IP>:30001/" -UseBasicParsing | Out-Null }
}
```

This increases CPU load → triggers HPA → scales pods automatically.

**Monitor scaling:**
```bash
kubectl get hpa -n swiggi-clone -w
```

---

## 🖥️ Access Points

| Component | URL / Port |
|-----------|------------|
| **Application** | `http://EC2-IP:30001` |
| **Grafana** | `http://EC2-IP:30002` |
| **Prometheus** | `http://EC2-IP:30003` |

---

## 📚 Tools & Technologies

- Kubernetes (Kind)
- Docker & Docker Hub
- AWS EC2
- Prometheus
- Grafana
- Metrics Server
- HPA (Horizontal Pod Autoscaler)
- NodePort Services
- YAML Manifests (Deployment, Service, Namespace, HPA)

---

## 📝 Project Highlights

✅ Full DevOps workflow on single EC2 instance  
✅ Auto-scaling application using HPA  
✅ Complete monitoring dashboard with Grafana  
✅ Real HPA testing with fake traffic  
✅ Clean Kubernetes manifests  
✅ Production-like architecture

---

## 🚀 Getting Started

### Prerequisites
- AWS EC2 instance (Ubuntu)
- Docker installed
- Kind installed
- kubectl installed
- Helm installed

### Quick Setup
```bash
# Pull the application image
docker pull daxdobariya/swiggi_clone

# Create Kind cluster with port mappings
kind create cluster --config=kind-config.yaml

# Create namespace
kubectl create namespace swiggi-clone

# Apply manifests
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f hpa.yaml

# Install monitoring
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
```

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 👨‍💻 Author

**Dax Dobariya**

Feel free to ⭐ this repository if you found it helpful!
