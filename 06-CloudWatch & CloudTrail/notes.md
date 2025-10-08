# ☁️ Day 06 – AWS CloudWatch & CloudTrail

---

## 🧠 Overview

**Amazon CloudWatch** and **AWS CloudTrail** are core AWS monitoring and auditing tools.

- **CloudWatch** helps monitor AWS resources and applications (metrics, logs, alarms, events, dashboards).  
- **CloudTrail** helps track user and API activity across your AWS account for governance and compliance.

Together, they ensure visibility, automation, and accountability in AWS.

---

## 🚀 CloudWatch – Monitoring Service

### 🔹 What is CloudWatch?
Amazon CloudWatch is a **monitoring and observability service** that collects:
- Metrics (CPU, Disk, Network)
- Logs (System & Application)
- Events (Triggers & Automation)
- Alarms (Threshold-based alerts)
- Dashboards (Visualization)

It monitors AWS resources in real time.

---

### 🔹 CloudWatch Components

| Component | Description |
|------------|--------------|
| **Metrics** | Numeric data points like CPUUtilization, NetworkIn |
| **Alarms** | Set thresholds to trigger notifications/actions |
| **Logs** | Central log storage from EC2, Lambda, ECS, etc. |
| **Events (Rules)** | Automate responses to system events |
| **Dashboards** | Custom visualization of metrics |
| **Agent / Unified Agent** | Collects system-level metrics from EC2 or on-prem servers |

---

### 🔹 Common AWS Metrics

| Service | Example Metrics |
|----------|------------------|
| **EC2** | CPUUtilization, DiskReadOps |
| **RDS** | CPUUtilization, FreeStorageSpace |
| **EBS** | VolumeReadOps, VolumeWriteOps |
| **Lambda** | Invocations, Duration, Errors |
| **S3** | BucketSizeBytes, NumberOfObjects |

---

### 🔹 CloudWatch Alarm States

- **OK** – Metric within threshold  
- **ALARM** – Metric outside threshold  
- **INSUFFICIENT_DATA** – Not enough data to evaluate  

---

### 🔹 CloudWatch Logs
- Collects application/system logs.
- Organized as:
  - **Log Groups** → contain multiple **Log Streams**
- You can create **metric filters** to trigger actions based on logs.

---

### 🔹 CloudWatch Dashboards
Custom dashboards to visualize metrics in one view — e.g., CPU, memory, and network usage.


### 🔹 CloudWatch Agent Setup (EC2)

  sudo yum install amazon-cloudwatch-agent -y
  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
  sudo systemctl start amazon-cloudwatch-agent
  sudo systemctl enable amazon-cloudwatch-agent

**🧾 CloudTrail – Auditing & Governance**
**🔹 What is CloudTrail?**

  AWS CloudTrail records all API calls and console actions for your AWS account.
  
  Captures who did what, when, and from where.
  
  Provides audit logs stored in S3.
  
  Integrates with CloudWatch and SNS for alerts.



