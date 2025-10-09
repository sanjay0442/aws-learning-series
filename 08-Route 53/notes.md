# 🌍 Day 08 – AWS Route 53 (DNS & Traffic Management)

---

## 🧠 What is Route 53?

**Amazon Route 53** is a **highly available and scalable Domain Name System (DNS) web service** by AWS.

It connects **user requests (like example.com)** to **AWS resources** (like EC2, S3, CloudFront, or Load Balancers).

---

## 💡 Why the Name “Route 53”?

- “Route” = routing traffic  
- “53” = the **default DNS port** (TCP/UDP port 53)

---

## ⚙️ Core Purpose

Route 53 helps you:
1. **Register domains** (like `mywebsite.com`)  
2. **Route Internet traffic** to AWS services (EC2, S3, ALB, CloudFront, etc.)  
3. **Perform health checks** and route only to healthy endpoints  
4. **Manage multi-region traffic** for global users

---

## 🏗️ Key Components

| Component | Description |
|------------|--------------|
| **Domain Registration** | You can buy domains directly from AWS (like a GoDaddy alternative). |
| **Hosted Zone** | A container that holds all DNS records for your domain (like example.com). |
| **Record Sets** | Individual DNS records that define how traffic is routed. |
| **Health Checks** | Monitors endpoint health and helps in failover routing. |

---

## 🧩 Understanding DNS Basics

When a user types:

https://www.example.com


1. The DNS resolver checks Route 53.  
2. Route 53 translates the domain name to an **IP address** (e.g., `54.213.25.7`).  
3. The browser connects to that IP to load the website.

---

## 🧱 Types of DNS Records in Route 53

| Record Type | Description | Example |
|--------------|--------------|----------|
| **A (Address)** | Maps a domain to an IPv4 address | `example.com → 192.0.2.1` |
| **AAAA** | Maps a domain to an IPv6 address | `example.com → 2001:db8::1` |
| **CNAME (Canonical Name)** | Maps one name to another | `www.example.com → example.com` |
| **MX (Mail Exchange)** | Routes email to mail servers | `example.com → mail.example.com` |
| **TXT** | Stores text data (SPF, DKIM, verification) | `"v=spf1 include:amazonses.com ~all"` |
| **NS (Name Server)** | Lists the authoritative name servers for the hosted zone | `ns-102.awsdns-20.com` |
| **SOA (Start of Authority)** | Holds admin info about the domain | Default AWS config |
| **Alias Record** | AWS-specific record type that routes traffic to AWS resources (no IP needed) | `example.com → ALB / S3 / CloudFront` |

---

## ☁️ Alias vs CNAME

| Feature | Alias | CNAME |
|----------|--------|--------|
| Supported for root domain (`example.com`) | ✅ Yes | ❌ No |
| AWS integration | ✅ Native (ALB, S3, CloudFront, etc.) | ❌ Not supported |
| Cost | Free queries | Standard query charge |

---

## 🧭 Hosted Zones

There are two types of hosted zones:

| Type | Description | Example |
|------|--------------|----------|
| **Public Hosted Zone** | Used for domains accessible on the Internet | `mywebsite.com` |
| **Private Hosted Zone** | Used inside a VPC for internal DNS resolution | `internal.mycompany.local` |

---

## 🌎 Route 53 Routing Policies

Routing policies decide **how Route 53 responds** to DNS queries.

| Policy Type | Description | Use Case |
|--------------|--------------|----------|
| **Simple Routing** | Returns a single record | For single web server setups |
| **Weighted Routing** | Distributes traffic based on weights (%) | A/B testing or blue-green deployments |
| **Latency-Based Routing** | Routes users to the region with lowest latency | Improve speed for global users |
| **Failover Routing** | Routes to a backup when primary fails | Disaster recovery setups |
| **Geolocation Routing** | Routes based on user’s physical location | Country/region-specific content |
| **Geoproximity Routing** | Routes based on user location **and** bias rules | Custom control over region routing |
| **Multivalue Answer Routing** | Returns multiple healthy records randomly | Basic load balancing without ELB |

---

## 💡 Example Use Case Scenarios

| Scenario | Routing Policy | Explanation |
|-----------|----------------|-------------|
| Single EC2 website | Simple | One web server handles all traffic |
| Blue-Green deployment | Weighted | 80% traffic → v1, 20% → v2 |
| Multi-region web app | Latency-based | Route India users to Mumbai region |
| DR setup | Failover | Primary (Mumbai), Backup (Singapore) |
| Regional site access | Geolocation | Indian users → india.example.com |
| Basic load balancing | Multivalue | Return multiple healthy IPs |

---

## 🧠 How Route 53 Works (Flow Diagram)


