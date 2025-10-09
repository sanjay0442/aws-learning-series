# 🧪 Day 11: AWS Well-Architected Framework – Hands-On Labs

In this section, you’ll **practically explore** how the AWS Well-Architected Framework helps improve your architecture.  
We’ll use the **AWS Well-Architected Tool (WAT)** and review each of the six pillars with practical examples.

---

## 🧭 Lab 1: Access the AWS Well-Architected Tool

### 🎯 Goal
To access and explore the AWS Well-Architected Tool and create your first workload review.

### 🧩 Steps
1. Go to the **AWS Management Console**.
2. Navigate to:  
   `Services → Management & Governance → Well-Architected Tool`
3. Click on **Create workload**.
4. Fill in the details:
   - **Workload name:** My-WebApp-Review  
   - **Environment:** Production  
   - **AWS Region:** Select your current region  
   - **Industry type:** Technology / Web  
   - **Review owner:** Your name or project team  
   - **Workload type:** Custom  
5. Click **Create workload**.

### 💡 Explanation
You’ve now created a workload review. This is where you assess your architecture using AWS best practices.

### ✅ Expected Result
A new workload appears in your Well-Architected dashboard, ready for pillar-wise review.

---

## ⚙️ Lab 2: Review the Operational Excellence Pillar

### 🎯 Goal
To identify improvements for operations and automation in your workload.

### 🧩 Steps
1. Open your workload → Select **Operational Excellence** pillar.
2. Read through each question carefully (e.g., “How do you design your workload to support evolution?”)
3. Select answers based on your setup (e.g., using **CloudFormation**, **CloudWatch**, **AWS Systems Manager**).
4. Add improvement notes where needed.
5. Click **Save and Exit**.

### 💡 Explanation
Operational Excellence focuses on continuous improvement and automation.  
You can track which practices are already implemented and which need work.

### ✅ Expected Result
You see recommendations like:
> “Automate deployment using CloudFormation or CDK.”  
> “Set up alarms in CloudWatch for proactive monitoring.”

---

## 🔐 Lab 3: Evaluate the Security Pillar

### 🎯 Goal
To identify gaps in IAM, data protection, and monitoring.

### 🧩 Steps
1. Select the **Security Pillar** in your workload.
2. Review questions such as:
   - “How do you manage user access?”
   - “How do you protect data at rest and in transit?”
3. Provide answers reflecting your environment.
4. Check AWS recommendations and note improvement actions.

### 💡 Explanation
Security reviews ensure encryption, IAM roles, logging, and least-privilege principles are followed.

### ✅ Expected Result
You receive guidance like:
> “Enable CloudTrail across all regions.”  
> “Use AWS KMS to encrypt S3 data.”

---

## ⚙️ Lab 4: Test Reliability Using Multi-AZ Deployment

### 🎯 Goal
To implement reliability best practices using real AWS services.

### 🧩 Steps
1. Launch an **RDS instance** with **Multi-AZ** enabled.  
2. Create an **EC2 Auto Scaling Group** in at least two **Availability Zones**.
3. Deploy a sample web app using a **Load Balancer**.
4. Stop one EC2 instance to simulate failure.

### 💡 Explanation
Multi-AZ and Auto Scaling ensure your application continues running even if one resource fails.

### ✅ Expected Result
The Load Balancer redirects traffic automatically to healthy instances — no downtime experienced.

---

## 💰 Lab 5: Implement Cost Optimization Best Practices

### 🎯 Goal
To monitor and reduce costs using AWS Cost tools.

### 🧩 Steps
1. Go to **Billing Dashboard → Cost Explorer** → Enable Cost Explorer.
2. Set up **AWS Budgets**:
   - Budget type: Cost
   - Budget name: Monthly-Budget
   - Amount: ₹1000 (or as desired)
   - Notification: Send email alert when 80% threshold is reached.
3. Review **Trusted Advisor** recommendations under the **Cost Optimization** category.

### 💡 Explanation
This helps you identify unused resources (e.g., idle EC2, unattached EBS volumes).

### ✅ Expected Result
Receive email alerts for cost thresholds and see optimization suggestions in Trusted Advisor.

---

## 🚀 Lab 6: Test Performance Efficiency with CloudFront

### 🎯 Goal
To enhance performance using caching and content delivery.

### 🧩 Steps
1. Upload a static website to an S3 bucket (e.g., index.html, images).
2. Enable **Static Website Hosting** on S3.
3. Create a **CloudFront Distribution**:
   - Origin: S3 bucket
   - Enable caching and HTTPS
4. Access your website using the CloudFront URL.

### 💡 Explanation
CloudFront caches content globally for low latency and high availability.

### ✅ Expected Result
Your website loads faster via CloudFront distribution instead of direct S3 link.

---

## 🌱 Lab 7: Implement Sustainability Best Practices

### 🎯 Goal
To apply sustainability principles in your workloads.

### 🧩 Steps
1. Use **Compute Optimizer** → identify underutilized instances.
2. Migrate to **Graviton-based EC2** (ARM) instances for better energy efficiency.
3. Implement **S3 Lifecycle Rules** to move infrequently accessed data to Glacier.
4. Delete unused resources.

### 💡 Explanation
Reducing unused resources and optimizing compute helps lower energy consumption and cost.

### ✅ Expected Result
Your account shows fewer idle resources, reduced cost, and lower carbon footprint.

---

## 🧰 Lab 8: Generate a Well-Architected Report

### 🎯 Goal
To export and analyze your architecture review.

### 🧩 Steps
1. Open your **Workload** in the Well-Architected Tool.
2. Click **Generate Report** → choose **PDF or JSON**.
3. Download and review improvement areas.
4. Share it with your team for implementation.

### 💡 Explanation
The report summarizes your architecture’s strengths and weaknesses based on AWS best practices.

### ✅ Expected Result
A downloadable report highlighting which pillars need attention and recommended improvements.

---

## 🧹 Lab 9: Cleanup

To avoid unnecessary charges:
- Delete RDS and EC2 instances used in labs.
- Remove CloudFront distributions.
- Delete unused S3 buckets and budgets.
- Terminate any created workloads if no longer needed.

---

## ✅ Summary

| Pillar | Lab Activity |
|--------|---------------|
| Operational Excellence | Created workload and automation recommendations |
| Security | IAM and encryption review |
| Reliability | Multi-AZ and Auto Scaling |
| Cost Optimization | AWS Budgets and Trusted Advisor |
| Performance Efficiency | S3 + CloudFront caching |
| Sustainability | Graviton + S3 Lifecycle |

---

📘 **Next Module → [Day 12: AWS Lambda & Serverless Concepts](../day12-lambda-serverless.md)**

