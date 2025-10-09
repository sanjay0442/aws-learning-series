# 💻 Day 10: AWS Backup & Cost Management – Hands-On Labs

This lab will help you practice **automated backup creation, restore, and cost monitoring** in AWS.  
You’ll use the **AWS Backup** service along with **Billing**, **Budgets**, and **Cost Explorer** tools.

---

## 🧪 Lab 1: Create a Backup Vault

### 🎯 Goal
To create a secure container where all backup recovery points will be stored.

### 🧭 Steps
1. Go to **AWS Console → AWS Backup → Backup Vaults → Create Backup Vault**  
2. Enter:
   - **Vault name:** `MyBackupVault`
   - **Encryption Key:** AWS managed key (default: `aws/backup`)
3. Click **Create Backup Vault**

### 💡 Explanation
A **Backup Vault** is like a folder that stores backup copies (recovery points).  
You can also apply access policies and encryption here.

✅ **Expected Result:**  
Vault `MyBackupVault` created successfully.

---

## 🧪 Lab 2: Create a Backup Plan

### 🎯 Goal
To define backup rules — when and how backups will run.

### 🧭 Steps
1. Go to **AWS Backup → Backup plans → Create backup plan**
2. Choose **Build a new plan**
3. Enter:
   - **Name:** `Daily-Backup-Plan`
   - **Backup rule name:** `DailyBackupRule`
   - **Backup frequency:** Daily
   - **Backup window:** Use default
   - **Lifecycle:** Transition to cold storage after 30 days, expire after 90 days
   - **Backup Vault:** `MyBackupVault`
4. Click **Create plan**

### 💡 Explanation
The plan defines **how often** backups happen and **how long** they are kept before deletion.

✅ **Expected Result:**  
A plan named `Daily-Backup-Plan` appears in the list.

---

## 🧪 Lab 3: Assign Resources to the Backup Plan

### 🎯 Goal
To attach AWS resources (like EC2 or RDS) to the backup plan.

### 🧭 Steps
1. Go to your **Backup Plan → Assign resources**
2. Enter:
   - **Resource assignment name:** `EC2-Backup-Assignment`
   - **IAM role:** Create new or use default (`AWSBackupDefaultServiceRole`)
   - **Resources:** Select one of your EC2 instances or EBS volumes
3. Click **Assign resources**

### 💡 Explanation
The IAM role allows AWS Backup to access and manage backups for those resources.

✅ **Expected Result:**  
Backup jobs are scheduled according to your plan.

---

## 🧪 Lab 4: Run On-Demand Backup

### 🎯 Goal
To create a manual backup immediately.

### 🧭 Steps
1. Go to **AWS Backup → Protected resources**
2. Click **Create on-demand backup**
3. Choose:
   - **Resource type:** EC2 or EBS
   - **Resource ID:** Select your instance or volume
   - **Vault:** `MyBackupVault`
4. Click **Create on-demand backup**

### 💡 Explanation
Manual backups are helpful before making critical configuration changes.

✅ **Expected Result:**  
A backup job runs and creates a recovery point under `MyBackupVault`.

---

## 🧪 Lab 5: Restore from Backup

### 🎯 Goal
To test restoration of a backed-up resource.

### 🧭 Steps
1. Go to **AWS Backup → Backup vaults → MyBackupVault**
2. Open a **Recovery point**
3. Click **Restore**
4. Choose the restore settings (e.g., EC2 instance type or volume)
5. Click **Restore backup**

### 💡 Explanation
AWS creates a **new resource** from the backup; it does not overwrite the existing one.

✅ **Expected Result:**  
A new EC2 instance or EBS volume is created successfully.

---

## 🧪 Lab 6: Create an AWS Budget (Cost Control)

### 🎯 Goal
To set a spending limit and receive alerts when costs exceed the threshold.

### 🧭 Steps
1. Go to **AWS Console → Billing → Budgets → Create budget**
2. Choose **Cost budget**
3. Enter:
   - **Name:** `Monthly-Budget`
   - **Period:** Monthly
   - **Budget amount:** Example ₹1000 or $10
4. Add **Alert threshold:** 80%
   - Notification email: your email ID
5. Click **Create budget**

### 💡 Explanation
Budgets help monitor spending and prevent cost overruns by sending alerts.

✅ **Expected Result:**  
You’ll receive an alert if your usage crosses 80% of ₹1000/$10.

---

## 🧪 Lab 7: Analyze Costs in Cost Explorer

### 🎯 Goal
To visualize AWS usage and cost trends.

### 🧭 Steps
1. Go to **Billing → Cost Explorer**
2. Enable Cost Explorer if not already
3. Use filters:
   - **Time range:** Last 3 months
   - **Group by:** Service
   - **Granularity:** Monthly
4. Click **Apply**

### 💡 Explanation
You’ll see which AWS services consume most of your budget — EC2, S3, Backup, etc.

✅ **Expected Result:**  
Cost graphs show clear spending trends per service.

---

## 🧪 Lab 8: Enable Billing Alerts in CloudWatch

### 🎯 Goal
To get CloudWatch notifications when your AWS bill exceeds a threshold.

### 🧭 Steps
1. Go to **CloudWatch → Alarms → Billing → Create Alarm**
2. Metric: `Billing → Total Estimated Charge`
3. Set threshold (e.g., > $5 or ₹400)
4. Create SNS topic → Add your email for notification
5. Confirm subscription via email
6. Click **Create Alarm**

### 💡 Explanation
CloudWatch monitors AWS billing and notifies you when spending increases suddenly.

✅ **Expected Result:**  
An email alert when your AWS bill exceeds the defined limit.

---

## 🧪 Lab 9: Clean-Up

### 🎯 Goal
Avoid unnecessary charges.

### 🧭 Steps
- Delete Backup Plans
- Delete Recovery Points and Vaults
- Remove IAM roles created by AWS Backup
- Delete Budgets or disable Cost Explorer if not needed

✅ **Expected Result:**  
All backup and budget resources cleaned up safely.

---

## ✅ Summary

| Lab | Description |
|------|--------------|
| Lab 1 | Create a Backup Vault |
| Lab 2 | Create Backup Plan |
| Lab 3 | Assign EC2 Resources |
| Lab 4 | Run Manual Backup |
| Lab 5 | Restore from Backup |
| Lab 6 | Create AWS Budget |
| Lab 7 | Use Cost Explorer |
| Lab 8 | Enable Billing Alerts |
| Lab 9 | Clean-up |

---

📘 **Next Module → [Day 11: AWS Well-Architected Framework](../notes/day11-well-architected.md)**  

