# 🛡️ Day 19: AWS Security (Theory)

## 1️⃣ Introduction to AWS Security
AWS Security is built around the **shared responsibility model** —  
AWS secures the **infrastructure**, while **you (the customer)** secure everything *inside* your AWS environment (data, access, configurations, etc.).

AWS provides a wide range of **security services** and **best practices** that help protect your workloads from threats and maintain compliance.

---

## 2️⃣ The Shared Responsibility Model (Recap)

| Layer                   | AWS Responsibility     | Customer Responsibility       |
| ----------------------- | ---------------------- | ----------------------------- |
| Physical Infrastructure | ✅ Secure data centers  | ❌                             |
| Network and Hardware    | ✅ AWS backbone network | ❌                             |
| Virtualization          | ✅ Hypervisors          | ❌                             |
| Operating System        | ❌                      | ✅ Patching & hardening        |
| Applications            | ❌                      | ✅ Secure coding & config      |
| IAM & Access Control    | ❌                      | ✅ Manage users, policies, MFA |
| Data Encryption         | ❌                      | ✅ Encrypt, manage keys        |
| Logging & Monitoring    | ❌                      | ✅ Review logs, alerting       |

---

## 3️⃣ Core AWS Security Services

Below are the **most important AWS security services**, grouped by functionality:

---

### 🔐 Identity and Access Management

| Service                                | Purpose                                              | Key Features                   |
| -------------------------------------- | ---------------------------------------------------- | ------------------------------ |
| **IAM (Identity & Access Management)** | Manage access to AWS resources                       | Users, Groups, Roles, Policies |
| **AWS SSO (Single Sign-On)**           | Centralized access control for multiple AWS accounts | Integrates with AD, Okta       |
| **AWS Organizations**                  | Manage multiple AWS accounts with policies           | SCP (Service Control Policies) |
| **Cognito**                            | User authentication for web/mobile apps              | OAuth, JWT, MFA support        |
| **STS (Security Token Service)**       | Temporary credentials                                | Used for cross-account access  |


---

### 🧱 Infrastructure & Network Protection

| Service                                | Purpose                                  | Key Features                              |
| -------------------------------------- | ---------------------------------------- | ----------------------------------------- |
| **AWS WAF (Web Application Firewall)** | Protect web apps from common attacks     | Blocks SQLi, XSS, IP filtering            |
| **AWS Shield**                         | DDoS protection                          | Standard (free) and Advanced (paid) tiers |
| **VPC Security**                       | Isolate and control traffic              | Security Groups, NACLs, Private Subnets   |
| **Firewall Manager**                   | Centralized WAF & Shield rule management | Multi-account policy enforcement          |


---

### 🔍 Threat Detection & Monitoring

| Service              | Purpose                                   | Key Features                                  |
| -------------------- | ----------------------------------------- | --------------------------------------------- |
| **Amazon GuardDuty** | Detect malicious or unauthorized activity | ML-based threat detection                     |
| **AWS Security Hub** | Unified security & compliance dashboard   | Integrates with GuardDuty, Inspector, Config  |
| **Amazon Inspector** | Automated vulnerability scanning          | Scans EC2 & ECR for CVEs                      |
| **Macie**            | Data protection for S3                    | Identifies PII (Personally Identifiable Info) |
| **CloudTrail**       | Logs every API call in your AWS account   | Used for auditing & incident investigation    |
| **CloudWatch**       | Monitors performance & generates alerts   | Logs, metrics, dashboards                     |


---

### 🔏 Data Protection & Encryption

| Service                              | Purpose                                             | Key Features                           |
| ------------------------------------ | --------------------------------------------------- | -------------------------------------- |
| **AWS KMS (Key Management Service)** | Centralized encryption key management               | CMK (Customer Master Keys), rotation   |
| **CloudHSM**                         | Hardware-based key storage                          | FIPS 140-2 compliance                  |
| **AWS Secrets Manager**              | Securely store & rotate secrets (passwords, tokens) | Automatic rotation, Lambda integration |
| **AWS Certificate Manager (ACM)**    | Manage SSL/TLS certificates                         | Free public & private certificates     |
| **S3 Encryption**                    | Server-side & client-side encryption                | SSE-S3, SSE-KMS, SSE-C                 |


---

### 🧩 Compliance, Audit, and Governance

| Service           | Purpose                                      | Key Features                                 |
| ----------------- | -------------------------------------------- | -------------------------------------------- |
| **AWS Config**    | Track resource configurations and compliance | Rules for auditing and drift detection       |
| **Audit Manager** | Automate evidence collection for audits      | Supports ISO, SOC2, PCI-DSS frameworks       |
| **Artifact**      | Download AWS compliance reports              | Self-service portal for certifications       |
| **Security Hub**  | Central compliance and security findings     | Maps findings to CIS, PCI-DSS, ISO standards |

---

## 4️⃣ AWS Security Workflow Example

Here’s how a typical **AWS Security Architecture** ties together:

1. **IAM + SSO** → Controls access  
2. **GuardDuty + Inspector** → Detects vulnerabilities or intrusions  
3. **CloudTrail + Config** → Logs and monitors all activities  
4. **Security Hub** → Centralizes findings and compliance alerts  
5. **KMS + Secrets Manager** → Encrypts and manages sensitive data  
6. **WAF + Shield** → Protects public-facing applications  

---

## 5️⃣ AWS Security Best Practices

✅ **Enable MFA** for all IAM users (especially root)  
✅ **Never use root credentials** for daily operations  
✅ **Rotate IAM access keys** regularly  
✅ **Enable CloudTrail in all regions**  
✅ **Use IAM roles** instead of embedding credentials in apps  
✅ **Restrict S3 buckets** (no public access unless required)  
✅ **Encrypt all data** (EBS, RDS, S3, etc.) using KMS  
✅ **Enable GuardDuty and Security Hub**  
✅ **Use least privilege** principle in IAM policies  
✅ **Use AWS Config** to detect misconfigurations  
✅ **Patch EC2 instances** regularly using Systems Manager Patch Manager  

---

## 6️⃣ Real-World Scenario

Imagine you run a web application on AWS:

- You secure login access using **Cognito + IAM roles**  
- Protect the frontend via **WAF + CloudFront**  
- Store data in **S3 with SSE-KMS encryption**  
- Monitor activity via **CloudWatch + CloudTrail**  
- Detect threats via **GuardDuty**  
- Get centralized compliance alerts via **Security Hub**  
- Regularly scan vulnerabilities with **Inspector**

Result → You’ve achieved **defense in depth** 🔐 — multiple layers of protection.

---

## 7️⃣ AWS Security Certifications (Optional Learning)

If you want to go deeper into AWS Security as a career path:

- 🎓 **AWS Certified Security – Specialty**  
  - Focus: threat detection, data protection, incident response  
- 🎓 **AWS Certified Advanced Networking – Specialty**  
  - Focus: secure network design, hybrid connectivity  
- 🎓 **AWS Certified Solutions Architect – Associate**  
  - Foundational for designing secure workloads  

---

## ✅ Summary

| Category           | Example Services                    |
| ------------------ | ----------------------------------- |
| Identity & Access  | IAM, SSO, STS, Cognito              |
| Network Protection | WAF, Shield, Firewall Manager       |
| Threat Detection   | GuardDuty, Inspector, Macie         |
| Data Protection    | KMS, Secrets Manager, ACM           |
| Compliance         | Security Hub, Config, Audit Manager |


AWS Security ensures your workloads are protected with **layered defense**, **continuous monitoring**, and **automated compliance**.

---

📘 **Next:**  
➡️ [Day 19 – AWS Security (Hands-On Labs)](../Day-19-AWS-Security-Labs)
