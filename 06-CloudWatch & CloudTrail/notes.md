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

****  🔹 CloudTrail Key Features****

Feature	                  Description
Event history:    	      Records all AWS API calls (90 days default)
Multi-region:             trails	Capture events across all regions
S3 integration:          	Store logs for long-term auditing
CloudTrail Insights:    	Detects unusual API activities
Integration:            	Works with CloudWatch Logs and SNS

**🔹 CloudTrail Event Types**
Type	                        Description
Management Events:          	Control-plane actions (CreateUser, StartInstances)
Data Events:                	Object-level activity (S3 GetObject, Lambda Invoke)
Insight Events:              	Detect unusual API spikes or anomalies

**Example CloudTrail Log**
  {
    "eventTime": "2025-10-04T12:45:00Z",
    "eventName": "StartInstances",
    "userIdentity": { "userName": "admin" },
    "sourceIPAddress": "115.241.56.33",
    "awsRegion": "ap-south-1"
  }

  **🏗️ CloudWatch vs CloudTrail**
  Feature	              CloudWatch	                      CloudTrail
  Purpose:            	Monitor performance	              Record user/API activity
  Data Type:          	Metrics, Logs	                    Event history
  Usage:             	  Resource health, performance	    Security auditing, compliance
  Storage:            	CloudWatch Logs                  	S3
  Integration:        	SNS, Lambda	                      CloudWatch Logs, SNS


  **🧩 Architecture Diagram (Text Representation)**
  

    ┌─────────────────────────────────────────────┐
  │                AWS ACCOUNT                  │
  ├─────────────────────────────────────────────┤
  │           CloudWatch (Monitoring)           │
  │  ├── Metrics (CPU, Memory, etc.)            │
  │  ├── Logs (EC2, Lambda)                     │
  │  ├── Alarms (Triggers SNS, AutoScaling)     │
  │  ├── Dashboards                             │
  │  └── Events (Automation Rules)              │
  │                                             │
  │           CloudTrail (Auditing)             │
  │  ├── Tracks API Calls & Console Actions     │
  │  ├── Stores Logs to S3                      │
  │  ├── Sends Notifications via SNS            │
  │  └── Detects Unusual Behavior               │
  └─────────────────────────────────────────────┘

  





