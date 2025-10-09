# 🧪 AWS Route 53 – Hands-On Labs (with Full Explanation)

Welcome to the Route 53 practical module!  
In these labs, you’ll learn to set up domain names, DNS records, health checks, and routing policies in a real AWS environment.  
No coding — just smart DNS configuration.

---

## ⚙️ Lab 1: Create a Public Hosted Zone

### 🎯 Goal:
Create a hosted zone for your domain (e.g., `myawsproject.com`) to manage DNS records in Route 53.

### 🧭 Steps:
1. Go to **AWS Console → Route 53 → Hosted Zones**
2. Click **Create hosted zone**
3. Enter:
   - **Domain name:** `myawsproject.com`
   - **Type:** Public hosted zone
4. Click **Create hosted zone**

### 💡 Explanation:
A **hosted zone** is like a container for all your DNS records (A, CNAME, etc.).  
You’ll now see four **Name Server (NS)** entries — these are your domain’s authoritative nameservers.

### ✅ Expected Result:
You have a new hosted zone in Route 53 for `myawsproject.com`, ready to store DNS records.

---

## ⚙️ Lab 2: Register or Connect a Domain

### 🎯 Goal:
Connect your hosted zone to a real domain.

### 🧭 Steps:
If you already own a domain (e.g., from GoDaddy or Namecheap):
1. Go to your **domain registrar’s DNS settings**
2. Replace their nameservers with the 4 NS entries from your Route 53 hosted zone

If you don’t have one:
- Go to **Route 53 → Registered Domains → Register Domain**
- Purchase a test domain (optional, costs apply)

### 💡 Explanation:
This step tells the internet:  
“Route 53 is the DNS manager for my domain.”

### ✅ Expected Result:
Your domain is now managed by AWS Route 53.

---

## ⚙️ Lab 3: Create Basic DNS Records (A, CNAME, MX, TXT)

### 🎯 Goal:
Add different record types and understand their purpose.

### 🧭 Steps:
Go to **Hosted Zone → myawsproject.com → Create Record**  
Create the following:

| Record Name | Type | Value | Purpose |
|--------------|------|--------|----------|
| (root) | A | 52.66.11.24 | Points to EC2/ALB IP |
| www | CNAME | myawsproject.com | Alias for main domain |
| mail | MX | 10 mail.myawsproject.com | Email routing |
| _amazonses | TXT | "v=spf1 include:amazonses.com ~all" | Email sender verification |

### 💡 Explanation:
Each record type serves a unique purpose:
- **A record** → Converts domain → IP
- **CNAME** → Points subdomain → domain
- **MX** → Mail server route
- **TXT** → Verification, SPF, DKIM, etc.

### ✅ Expected Result:
Your hosted zone now includes multiple records for web, email, and verification use.

---

## ⚙️ Lab 4: Alias Record for AWS Resources

### 🎯 Goal:
Map your domain to an AWS resource (like an S3 static website or ALB) using an Alias record.

### 🧭 Steps:
1. Create an S3 static website or Application Load Balancer.
2. Copy the **S3 website endpoint** or **ALB DNS name.**
3. In Route 53 → Hosted Zone → Create Record:
   - Record name: `www`
   - Record type: **A – Alias**
   - Alias target: Choose your S3/ALB endpoint
   - Routing policy: Simple
4. Click **Create Record**

### 💡 Explanation:
Alias records are **AWS-only CNAMEs** — faster and cheaper.
They allow root domains (e.g., `myawsproject.com`) to point directly to AWS resources.

### ✅ Expected Result:
Visiting `www.myawsproject.com` should open your S3 website or ALB application.

---

## ⚙️ Lab 5: Health Checks and Failover Routing

### 🎯 Goal:
Route traffic automatically to a healthy resource (e.g., backup server).

### 🧭 Steps:
1. Go to **Route 53 → Health Checks → Create Health Check**
   - Name: `Primary-EC2-HealthCheck`
   - Endpoint: IP or domain of your EC2
   - Protocol: HTTP
   - Path: `/`
2. Create two EC2 instances (Primary and Backup).
3. In Hosted Zone → Create Record:
   - Record 1:  
     - Name: `web.myawsproject.com`  
     - Type: A  
     - Value: Primary EC2 IP  
     - Routing policy: **Failover (Primary)**  
     - Health check: Attach `Primary-EC2-HealthCheck`
   - Record 2:  
     - Value: Backup EC2 IP  
     - Routing policy: **Failover (Secondary)**

### 💡 Explanation:
If the **Primary EC2** fails health checks, traffic automatically reroutes to the **Backup EC2.**

### ✅ Expected Result:
Failover works — when primary fails, Route 53 automatically sends users to backup.

---

## ⚙️ Lab 6: Weighted Routing Policy (A/B Testing)

### 🎯 Goal:
Distribute traffic between two environments (e.g., Version 1 and Version 2 of an app).

### 🧭 Steps:
1. Create two EC2s or ALBs for the same app (v1 and v2)
2. In Route 53 → Hosted Zone → Create Record:
   - Record 1:  
     - Name: `app.myawsproject.com`  
     - Type: A  
     - Value: v1 ALB DNS  
     - Routing policy: **Weighted (80%)**
   - Record 2:  
     - Name: `app.myawsproject.com`  
     - Type: A  
     - Value: v2 ALB DNS  
     - Routing policy: **Weighted (20%)**
3. Save both records.

### 💡 Explanation:
You can control what percentage of traffic goes to each version — perfect for blue-green or canary deployments.

### ✅ Expected Result:
80% of users hit v1, and 20% hit v2 of your application.

---

## ⚙️ Lab 7: Latency-Based Routing (Multi-Region Setup)

### 🎯 Goal:
Route users to the nearest region for faster response times.

### 🧭 Steps:
1. Deploy EC2 or ALB in two regions (e.g., Mumbai & Singapore)
2. Create two records for the same name (`app.myawsproject.com`):
   - Record 1 → Mumbai ALB → Region: ap-south-1
   - Record 2 → Singapore ALB → Region: ap-southeast-1
   - Routing policy: **Latency**
3. Test from India and Singapore (use VPN for testing).

### 💡 Explanation:
Route 53 automatically directs users to the AWS region with the **lowest network latency.**

### ✅ Expected Result:
Indian users connect to Mumbai, while Singapore users connect to Singapore ALB.

---

## ⚙️ Lab 8: Private Hosted Zone (Internal DNS)

### 🎯 Goal:
Use Route 53 for private internal DNS resolution within a VPC.

### 🧭 Steps:
1. Go to Route 53 → **Create Hosted Zone**
2. Enter:
   - Domain name: `internal.myawsproject.local`
   - Type: **Private hosted zone**
3. Associate this hosted zone with your **VPC**
4. Create an A record:
   - Name: `db.internal.myawsproject.local`
   - Value: `10.0.2.10` (private IP of RDS or EC2)

### 💡 Explanation:
Private hosted zones are only resolvable **within your VPC**, not on the Internet.

### ✅ Expected Result:
From within the VPC, ping or curl `db.internal.myawsproject.local` → resolves to private IP.

---

## ⚙️ Lab 9: Cleanup

### 🧭 Steps:
1. Delete records (except NS and SOA)
2. Delete hosted zones
3. Release Elastic IPs
4. Terminate EC2s
5. Remove health checks

### ✅ Result:
All Route 53 resources removed → no ongoing charges.

---

## 🧩 Summary

| Concept | Description |
|----------|--------------|
| **Hosted Zone** | DNS container for your domain |
| **Alias Record** | AWS-native record pointing to resources |
| **Health Check** | Monitors endpoints and supports failover |
| **Routing Policies** | Decide how Route 53 responds (Simple, Weighted, Latency, Failover, Geo, Multi-value) |
| **Private Hosted Zone** | Used for internal DNS resolution inside VPCs |

---

**Next Module → [Day 09: CloudFormation (Infrastructure as Code Basics)](../09-CloudFormation/README.md)**  

