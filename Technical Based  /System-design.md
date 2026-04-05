### 1. How would you design a scalable and highly available web application architecture?

<details>

## 🎯 Start with this:

"When I design for scalability and high availability, I think in layers — every single layer must be redundant, no single point of failure anywhere."

---

## 🏗️ Architecture I would explain:

👥 Users  
⬇️  
🌐 CDN (CloudFront / Azure CDN)        ← Static content, DDoS protection  
⬇️  
⚖️ Load Balancer (ALB / Azure LB)      ← Traffic distribution, SSL termination  
⬇️  
📈 Auto Scaling Group / VMSS           ← Scale in/out based on load  
⬇️  
⚙️ Application Servers (AKS/EKS)       ← Stateless app containers  
⬇️  
⚡ Cache Layer (Redis)                 ← Reduce DB hits  
⬇️  
🗄️ Database (Primary + Read Replicas)  ← Multi-AZ, automatic failover  
⬇️  
📦 Object Storage (Blob/S3)            ← Static files, backups  

---

## 💡 Real example to say:

"In our production setup, we deployed the application on AKS across 3 availability zones. Azure Load Balancer distributed traffic, Redis handled sessions, and Azure SQL had geo-redundant replication enabled. Even when one zone had an issue, users experienced zero downtime. Our SLA was 99.9% and we consistently hit 99.95%."

---

## 🧠 Close with this:

"The golden rule I follow — design for failure. Assume everything will fail at some point, and make sure the system handles it gracefully without user impact."

</details>

### 2. How do you design a CI/CD pipeline for a microservices-based application?

<details>

## 🎯 Start with this:

"For microservices CI/CD, the key principle is — each service must have its own independent pipeline. They should be deployable separately without affecting other services."

---

## 🔄 Pipeline flow I would design:

👨‍💻 Developer pushes code  
⬇️  

### 🧪 CI Stage

- 📥 Code Checkout  
- ✅ Unit Tests + Code Coverage  
- 🔍 Static Code Analysis (SonarQube)  
- 🔐 Security Scan (Snyk/Trivy)  
- 🐳 Build Docker Image  
- 📦 Push to Container Registry (ACR)  

⬇️  

### 🚀 CD Stage

- 🧪 Deploy to DEV  → Smoke Tests  
- 🔗 Deploy to QA   → Integration Tests  
- ⚡ Deploy to UAT  → Performance Tests  
- ✅ Manual Approval Gate  
- 🌍 Deploy to PROD → Health Check  

---

## 💡 Real example to say:

"We had 12 microservices, each with its own Azure DevOps pipeline. Every pipeline had a quality gate — if code coverage dropped below 80% or Trivy found a critical vulnerability, the pipeline failed automatically. Production deployments used Blue-Green strategy — zero downtime, instant rollback if health checks failed. We went from monthly releases to deploying 15 times a day safely."

---

## 🧠 Close with this:

"A good CI/CD pipeline is not just about automation — it's about building confidence. Every stage must give you a higher level of confidence that the code is production-ready."

</details>

### 3. How do you design a Kubernetes-based deployment architecture?

<details>

## 🎯 Start with this:

"When designing Kubernetes architecture, I think about four things — how traffic comes in, how apps are organized, how they scale, and how they stay secure."

---

## 🏗️ Architecture I would design:

🌐 Internet  
⬇️  
🚪 Azure Application Gateway / NGINX Ingress  
⬇️  

☸️ Kubernetes Cluster (AKS)  
├── 📂 Namespaces (dev / staging / prod)  
│     ├── 🚀 Deployments (stateless apps)  
│     ├── 🗄️ StatefulSets (databases, queues)  
│     ├── 📡 DaemonSets (logging, monitoring agents)  
│     └── ⏰ CronJobs (scheduled tasks)  
├── 📈 HPA (Horizontal Pod Autoscaler)  
├── 🖥️ Node Autoscaler (cluster-autoscaler)  
├── 🔐 Network Policies (pod-to-pod security)  
├── 👤 RBAC (who can do what)  
└── 🔑 Secrets Store CSI (Azure Key Vault integration)  

---

## 🚀 Deployment strategy I use:

strategy:  
  type: RollingUpdate  
  rollingUpdate:  
    maxSurge: 1        # 1 extra pod during update  
    maxUnavailable: 0  # zero downtime guaranteed  

---

## 💡 Real example to say:

"We designed AKS with 3 node pools — system pool for Kubernetes components, application pool with auto-scaling from 3 to 20 nodes, and a spot instance pool for batch jobs to save 70% cost. Each team had their own namespace with RBAC — dev team couldn't touch production namespace. Ingress handled SSL termination and path-based routing for 15 microservices. HPA kept pods at optimal count based on CPU and custom queue-depth metrics."

---

## 🧠 Close with this:

"A well-designed Kubernetes architecture should feel invisible to developers — they just push code and the platform handles everything else. That's the goal."

</details>
