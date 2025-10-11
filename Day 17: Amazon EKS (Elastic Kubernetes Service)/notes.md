# ☸️ Day 17: Amazon EKS (Elastic Kubernetes Service) – Theory Notes

## 🧠 What is Amazon EKS?

**Amazon Elastic Kubernetes Service (EKS)** is a **fully managed Kubernetes service** by AWS.  
It allows you to deploy, manage, and scale containerized applications using Kubernetes **without having to install or maintain the control plane (master nodes)**.

In short:
> EKS = Managed Kubernetes on AWS.

---

## ⚙️ Why Use EKS?

Kubernetes is powerful but complex to set up — EKS simplifies this by:
- Automatically managing Kubernetes **control plane & etcd database**
- Integrating with AWS services (EC2, Fargate, IAM, VPC, CloudWatch)
- Ensuring high availability and auto-scaling
- Providing **security, patching, and upgrades** automatically

---

## 🧩 Core Components of EKS

| Component | Description |
|------------|--------------|
| **Control Plane** | Managed by AWS. Handles scheduling, scaling, and API requests. |
| **Worker Nodes** | Your EC2 or Fargate instances where containers actually run. |
| **EKS Cluster** | The main Kubernetes cluster environment that connects all resources. |
| **Node Group** | A collection of EC2 instances registered as worker nodes. |
| **kubectl** | Command-line tool to interact with your EKS cluster. |
| **IAM Roles** | Used to control permissions for cluster and worker nodes. |
| **VPC** | Networking layer for pods, services, and node communication. |

---

## 🧠 How EKS Works (Simplified Flow)

1. You **create an EKS cluster** (AWS sets up control plane).
2. You **create worker nodes** (EC2 or Fargate).
3. Nodes **register** with the cluster.
4. You **deploy applications** using Kubernetes manifests (YAML files).
5. **EKS manages scaling, networking, and updates** automatically.

---

## 🏗️ EKS Architecture Overview

| AWS Cloud (EKS)                            |
| ------------------------------------------ |
| Control Plane (Managed by AWS)             |
| - API Server                               |
| - etcd Database                            |
| - Scheduler, Controller Manager            |
| ----------------------------------------   |
| Worker Nodes (EC2/Fargate)                 |
| - kubelet                                  |
| - kube-proxy                               |
| - Pods / Containers                        |
| +----------------------------------------+ |


✅ You manage the **nodes & pods**, AWS manages the **control plane**.

---

## 🌐 EKS Networking (VPC Integration)

EKS integrates deeply with your AWS VPC:
- Each **pod** gets its own **Elastic Network Interface (ENI)**.
- Network traffic between pods and services stays **inside your VPC**.
- You can use **Security Groups** and **NACLs** to control access.

---

## 🔐 EKS Security Overview

| Security Layer | AWS Service / Concept |
|----------------|------------------------|
| IAM Roles | Control who can access the cluster (RBAC + IAM integration) |
| Security Groups | Define inbound/outbound rules for worker nodes |
| Secrets Manager | Store Kubernetes secrets securely |
| AWS WAF & Shield | Protect exposed applications |
| CloudTrail | Track API calls made to EKS |

---

## 🚀 EKS Deployment Models

1. **EKS on EC2**
   - You manage and maintain the EC2 worker nodes.
   - Best for custom configurations and workloads needing full control.

2. **EKS on Fargate**
   - Serverless option — no need to manage EC2 nodes.
   - AWS handles provisioning and scaling containers.

3. **EKS Anywhere**
   - Deploy Kubernetes clusters on-premises or other clouds.
   - Useful for hybrid environments.

---

## ⚖️ EKS vs ECS (Quick Comparison)

| Feature | ECS | EKS |
|----------|-----|-----|
| Type | AWS proprietary container orchestration | Kubernetes-based orchestration |
| Control Plane | Managed by AWS | Managed by AWS |
| Compatibility | AWS-only | Kubernetes standard (multi-cloud) |
| Learning Curve | Easier | Steeper |
| Customization | Limited | Highly flexible |
| Use Case | Simple container workloads | Complex microservices architecture |

---

## 🔄 EKS Lifecycle Management

| Action | Tool / Command |
|--------|----------------|
| Create Cluster | AWS Console / CLI / eksctl |
| Connect Cluster | `aws eks update-kubeconfig` |
| Deploy Apps | `kubectl apply -f deployment.yaml` |
| Monitor Logs | CloudWatch |
| Scale Pods | `kubectl scale` |
| Delete Cluster | `eksctl delete cluster` |

---

## 🧰 Tools Commonly Used with EKS

| Tool | Purpose |
|------|----------|
| **eksctl** | Simplified CLI tool for creating/managing EKS clusters |
| **kubectl** | Kubernetes CLI for interacting with clusters |
| **Helm** | Package manager for Kubernetes (deploy apps easily) |
| **CloudFormation / Terraform** | Automate EKS infrastructure setup |
| **CloudWatch** | Monitor cluster logs and metrics |

---

## 📋 Common EKS Use Cases

- Microservices-based applications
- Machine Learning workloads
- CI/CD pipelines with Kubernetes
- Hybrid or multi-cloud deployments
- Real-time streaming and data analytics apps

---

## ❓ Common Interview Questions

1. What is EKS, and how does it differ from ECS?
2. What’s the role of the control plane in EKS?
3. How do worker nodes communicate with the control plane?
4. What is `eksctl`, and why is it used?
5. How does EKS integrate with IAM and VPC?
6. Can EKS run without EC2 instances?
7. What are some common monitoring tools for EKS?

---

## 🧩 Summary

| Concept | Description |
|----------|--------------|
| EKS | Managed Kubernetes service by AWS |
| Worker Node | EC2/Fargate instance running pods |
| eksctl | CLI to manage EKS clusters easily |
| IAM + RBAC | Secure cluster access |
| CloudWatch | Logs and metrics for EKS |
| ECR | Container image storage integrated with EKS |

---

## 🏁 Next Step

👉 Proceed to **Day 17 Labs: Amazon EKS Hands-on**  
You’ll create an EKS cluster, deploy workloads, connect using `kubectl`, and integrate with ECR.
