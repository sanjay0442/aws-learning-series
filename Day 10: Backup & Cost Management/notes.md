# 🧠 Day 10: AWS Backup & Cost Management

## 📘 1️⃣ What is AWS Backup?

**AWS Backup** is a **fully managed service** that allows you to **centrally manage and automate data backups** across multiple AWS services like:

- Amazon EC2  
- Amazon EBS  
- Amazon RDS  
- Amazon DynamoDB  
- Amazon EFS  
- Amazon FSx  

It simplifies backup management and ensures **data protection**, **compliance**, and **disaster recovery** across AWS workloads.

---

## 💡 2️⃣ Why Backups Are Important?

Backups protect your data from:

- Accidental deletion  
- Ransomware or corruption  
- Region-wide outages  
- Human errors  

They are essential for **Disaster Recovery (DR)** and **Business Continuity**.

---

## 🧩 3️⃣ AWS Backup Key Components

| Component | Description |
|------------|-------------|
| **Backup Vault** | Logical container where all backup recovery points are stored. |
| **Backup Plan** | Defines the schedule, retention period, and destination for backups. |
| **Backup Rule** | A rule within a plan that defines frequency and lifecycle actions. |
| **Resource Assignment** | The specific AWS resources (like EC2, RDS, etc.) included in the plan. |
| **Recovery Point** | The actual backup data copy that can be restored later. |

---

## 💾 4️⃣ Types of Backups

| Type | Description |
|-------|-------------|
| **On-Demand Backup** | Manual backup whenever needed. |
| **Scheduled Backup** | Automated backup based on defined schedule. |
| **Incremental Backup** | Backs up only changed data since the last backup — efficient and cost-saving. |

---

## 🔁 5️⃣ Restore Operations

When restoring, AWS creates a **new resource** (for example, a new EC2 volume or RDS instance) from the backup.  
You can restore:

- To the **same region**, or  
- To a **different region** (cross-region restore enabled).

This ensures data availability even during regional outages.

---

## 💰 6️⃣ AWS Cost Management Overview

AWS provides several tools to help you **track and optimize spending**:

| Tool | Description |
|-------|-------------|
| **AWS Billing Console** | View monthly bills and usage summaries. |
| **AWS Cost Explorer** | Analyze spending patterns and visualize trends. |
| **AWS Budgets** | Set custom cost or usage budgets and get alerts. |
| **AWS Cost Anomaly Detection** | Detect unusual cost spikes using ML. |
| **AWS Trusted Advisor** | Offers cost-saving and performance optimization recommendations. |

---

## 🧠 7️⃣ Cost Optimization Best Practices

✅ **Use Cost Explorer:** Identify top cost-driving services.  
✅ **Set Budgets & Alerts:** Notify when spending exceeds limits.  
✅ **Use Reserved Instances / Savings Plans:** Save up to 70% for long-term workloads.  
✅ **Use Spot Instances:** For flexible, fault-tolerant workloads.  
✅ **Implement S3 Lifecycle Policies:** Move infrequently accessed data to Glacier.  
✅ **Turn Off Idle Resources:** Stop unused EC2 or RDS instances.  
✅ **Use Compute Optimizer:** Get right-size recommendations for instances.  

---

## 💡 8️⃣ AWS Free Tier & Billing Alerts

- AWS offers **12 months of Free Tier**, e.g.:
  - 750 hours of EC2 t2.micro
  - 5GB S3 storage  
  - 750 hours of RDS micro instance

- You can configure **Billing Alerts** using **CloudWatch → Billing Metrics** to get notified when spending exceeds thresholds.

---

## ✅ Summary

| Feature | Purpose |
|----------|----------|
| **AWS Backup** | Unified backup automation across AWS resources. |
| **Backup Plan** | Define backup frequency, retention, and destination. |
| **Cost Explorer** | Visualize and analyze cost trends. |
| **Budgets** | Get alerts for overspending. |
| **Optimization** | Use RIs, Spot, S3 lifecycle, and stop idle resources. |

---

## 🚀 Next Step

Proceed to the [Hands-On Labs](../labs/day10-backup-cost-labs.md)  
to practice creating:
- A Backup Vault & Backup Plan  
- Assigning EC2/EBS resources  
- Configuring AWS Budgets and Cost Explorer  

---

