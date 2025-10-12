# 🧪 Day 19: AWS Security – Hands-On Labs

These labs will help you **practically implement AWS Security best practices** — covering IAM, GuardDuty, Inspector, CloudTrail, KMS, and WAF.

---

## 🔹 Lab 1: Enable AWS GuardDuty

### 🧭 Objective
Enable **Amazon GuardDuty** to detect unauthorized or malicious activity within your AWS account.

### 🧩 Steps

1. Go to **AWS Console → GuardDuty**
2. Click **Enable GuardDuty**
3. Wait for initialization (it may take a few minutes)
4. After activation:
   - GuardDuty automatically begins analyzing CloudTrail, DNS, and VPC Flow Logs
5. Click **Findings** in the sidebar to view detections

### ✅ Verify
- GuardDuty status = **Enabled**
- Findings = Auto-generated within 24 hours or via sample events:

Actions → Generate sample findings


---

## 🔹 Lab 2: Enable AWS Inspector (Vulnerability Scanning)

### 🧭 Objective
Use **Amazon Inspector** to scan EC2 instances and ECR repositories for vulnerabilities (CVEs).

### 🧩 Steps

1. Navigate to **Amazon Inspector** console  
2. Click **Enable Amazon Inspector**
3. Select **Activate for all accounts** (if using Organizations)
4. Once activated:
 - Inspector automatically scans all running EC2 instances and ECR images

5. Check findings under:

Amazon Inspector → Findings


### ✅ Verify
- Findings show CVE results (e.g., outdated packages)
- Each finding includes severity, CVE ID, and remediation steps

---

## 🔹 Lab 3: Enable CloudTrail for Account Activity Logging

### 🧭 Objective
Record and monitor every API call made in your AWS account using **AWS CloudTrail**.

### 🧩 Steps

1. Navigate to **CloudTrail** → **Trails**  
2. Click **Create trail**
3. Enter a name, e.g., `org-account-trail`
4. Select:
- **Apply trail to all regions** ✅
- **Create a new S3 bucket** for logs
5. Enable **CloudWatch Logs** integration (optional)
6. Click **Create trail**

### ✅ Verify
- Go to the S3 bucket → check for CloudTrail logs folder
- Confirm that API events (like IAM or EC2 activity) are logged

---

## 🔹 Lab 4: Enable AWS Config (Compliance Monitoring)

### 🧭 Objective
Track AWS resource configuration changes and ensure compliance.

### 🧩 Steps

1. Navigate to **AWS Config**
2. Click **Get started**
3. Choose:
- **Record all resources**
- **Include global resources** ✅
4. Create an **S3 bucket** for Config logs
5. Enable **AWS Config Rules** → Choose:
- `restricted-ssh` (ensures ports 22 aren’t public)
- `s3-bucket-public-read-prohibited`

### ✅ Verify
- Go to Config → **Resources**
- Check which ones are **Compliant** or **Non-compliant**
- You’ll receive detailed remediation advice

---

## 🔹 Lab 5: Enable AWS Security Hub (Centralized Security Dashboard)

### 🧭 Objective
Aggregate security findings from GuardDuty, Inspector, Config, and others.

### 🧩 Steps

1. Go to **AWS Security Hub**
2. Click **Enable Security Hub**
3. Once enabled:
- Integrate services (GuardDuty, Inspector, Macie, etc.)
4. Enable frameworks:
- CIS AWS Foundations Benchmark
- PCI DSS
5. Wait for a few minutes to populate findings.

### ✅ Verify
- Dashboard shows consolidated alerts
- Findings linked to other services (e.g., Inspector → Vulnerabilities)

---

## 🔹 Lab 6: Encrypt Data Using AWS KMS

### 🧭 Objective
Create and use a **KMS Customer Managed Key (CMK)** to encrypt data.

### 🧩 Steps

1. Navigate to **AWS KMS → Customer managed keys → Create key**
2. Choose:
- Key type: **Symmetric**
- Usage: **Encrypt and decrypt**
3. Add administrators (IAM users who can manage the key)
4. Define users who can use the key for encryption
5. Create the key

Now, encrypt data:
```bash
aws kms encrypt \
--key-id <your-key-id> \
--plaintext "HelloSecureWorld" \
--output text \
--query CiphertextBlob


Decrypt data:

aws kms decrypt \
  --ciphertext-blob fileb://<your-encrypted-file> \
  --output text \
  --query Plaintext | base64 --decode


✅ Verify

Data decrypts correctly with KMS key permissions

🔹 Lab 7: Configure AWS WAF (Web Application Firewall)
🧭 Objective

Protect your web application from attacks like SQL Injection and XSS.

🧩 Steps

Go to AWS WAF & Shield → Web ACLs

Click Create Web ACL

Select your resource (CloudFront, ALB, or API Gateway)

Add rules:

AWS Managed Rules → “Common Rule Set”

Add custom rule → Block IP range (optional)

Review and create

✅ Verify

WAF is attached to your web resource

Test by visiting blocked IP → should get Access Denied

🔹 Lab 8: Use AWS Secrets Manager for Storing Credentials
🧭 Objective

Store and retrieve a database password securely.

🧩 Steps

Go to Secrets Manager → Store a new secret

Choose:

Secret type: Other type of secret

Add key-value pairs:

username: admin
password: myStrongPass@123

Name your secret, e.g., db/admin/credentials

Choose Encryption key (KMS default or custom)

Click Store

Retrieve secret via CLI:

aws secretsmanager get-secret-value \
  --secret-id db/admin/credentials \
  --query SecretString \
  --output text

✅ Verify

Output shows your stored secret (encrypted at rest)

🔹 Lab 9: Setup AWS Shield for DDoS Protection
🧭 Objective

Enable protection against Distributed Denial-of-Service (DDoS) attacks.

🧩 Steps

AWS Shield Standard is enabled by default

To enable Shield Advanced:

Go to AWS Shield Console

Choose Subscribe to Shield Advanced

Select resource (ALB, CloudFront, Route 53)

Shield Advanced integrates with WAF and Firewall Manager

✅ Verify

Protected resources listed under “AWS Shield → Protected Resources”

🔹 Lab 10: Enable MFA for Root Account
🧭 Objective

Secure the AWS root account with Multi-Factor Authentication.

🧩 Steps

Go to IAM Dashboard → Security Recommendations

Under “Root user MFA,” click Activate MFA

Choose:

Virtual MFA (e.g., Google Authenticator, Authy)

Scan QR code → Enter 2 consecutive MFA codes

Confirm to enable

✅ Verify

IAM Dashboard shows “Root account MFA: Enabled ✅”

✅ Summary of Labs

| Lab | Service         | Purpose                    |
| --- | --------------- | -------------------------- |
| 1   | GuardDuty       | Threat detection           |
| 2   | Inspector       | Vulnerability management   |
| 3   | CloudTrail      | API activity logging       |
| 4   | Config          | Compliance tracking        |
| 5   | Security Hub    | Unified security dashboard |
| 6   | KMS             | Data encryption            |
| 7   | WAF             | Web app firewall           |
| 8   | Secrets Manager | Secret storage             |
| 9   | Shield          | DDoS protection            |
| 10  | MFA             | Account-level security     |

------------------------------------------------------------------------------------------------------------

**🚀 Challenge Task (Optional)**

Use AWS Config Rules to enforce encryption on all S3 buckets.

Integrate GuardDuty findings with AWS Lambda to auto-remediate compromised instances.

Create a KMS-encrypted EBS volume and attach it to an EC2 instance.
