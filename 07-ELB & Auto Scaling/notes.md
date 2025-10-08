# ⚙️ Day 07 – Elastic Load Balancer (ELB) & Auto Scaling

---

## 🧠 Overview

**Elastic Load Balancer (ELB)** and **Auto Scaling** work together to ensure:
- High availability  
- Fault tolerance  
- Scalability  

ELB automatically distributes incoming traffic, while Auto Scaling ensures you always have the **right number of EC2 instances** running.

---

## 🚀 Part 1: Elastic Load Balancer (ELB)

### 🔹 What is ELB?
Elastic Load Balancing automatically distributes incoming traffic across multiple targets (EC2, containers, IPs) in one or more Availability Zones (AZs).

---

### 🔹 Types of Load Balancers

| Type | Description | Use Case |
|------|--------------|----------|
| **Application Load Balancer (ALB)** | Operates at Layer 7 (HTTP/HTTPS). Supports routing based on host/path, WebSockets, and microservices. | Modern web apps, containers |
| **Network Load Balancer (NLB)** | Operates at Layer 4 (TCP/UDP). Ultra-low latency and high performance. | Gaming, IoT, real-time apps |
| **Gateway Load Balancer (GWLB)** | Operates at Layer 3. Used for third-party appliances like firewalls and IDS/IPS. | Network appliances |
| **Classic Load Balancer (CLB)** | Old version, operates at Layer 4 & 7. | Legacy apps only |

---

### 🔹 Key ELB Features

| Feature | Description |
|----------|-------------|
| **Health Checks** | ELB checks instance health and routes traffic only to healthy targets |
| **Cross-Zone Load Balancing** | Evenly distributes traffic across all AZs |
| **Sticky Sessions** | Keeps user session on the same instance |
| **SSL Termination** | Manages SSL certificates for HTTPS |
| **Access Logs** | Captures detailed request logs |
| **Target Groups** | Logical grouping of EC2 instances or services |

---

### 🔹 ELB Architecture


    ┌───────────────────────┐
    │      Internet Users   │
    └────────────┬──────────┘
                 │
          ┌──────▼───────┐
          │ Application  │
          │ Load Balancer│
          └──────┬───────┘
     ┌───────────┴───────────┐
     │                       │
     ┌───────▼────────┐     ┌───────▼────────┐
│ EC2 Instance 1 │          │ EC2 Instance 2 │
│ (us-east-1a) │ │         (us-east-1b) │
└─────────────────┘         └─────────────────┘


  ---
  
  ### 🔹 ELB Health Check Example
  - Protocol: HTTP  
  - Path: `/healthcheck.html`  
  - Threshold: 2 successes, 5 failures  
  
  If instance fails health check → ELB stops routing traffic to it.
  
  ---
  
  ### 🔹 ELB Listener Example
  - Port 80 (HTTP) → Target Group (EC2 Instances)  
  - Port 443 (HTTPS) → Target Group (EC2 Instances with SSL)
  
  ---
  
  ### 🔹 ELB Access Logs
  Stored in S3 for analysis.
  
  ```bash
  s3://my-elb-logs/AWSLogs/123456789012/elasticloadbalancing/

**🧩 Hands-On: Create Application Load Balancer (ALB)**

🔧 Steps

Launch 2 EC2 instances (Amazon Linux) in different AZs.

Install Apache:

  sudo yum install httpd -y
  echo "Hello from EC2-1" > /var/www/html/index.html
  sudo systemctl start httpd


Repeat for EC2-2 with message Hello from EC2-2.

Go to EC2 → Load Balancers → Create Load Balancer → Application Load Balancer.

Add both EC2s to a Target Group.

Create Listener on port 80 (HTTP).

Test by visiting ALB DNS name.

✅ Result: Requests alternate between EC2-1 and EC2-2.

🚀 Part 2: Auto Scaling
🔹 What is Auto Scaling?

Auto Scaling automatically adjusts the number of EC2 instances based on traffic or performance metrics.

Ensures:

Minimum number of instances (for reliability)

Scales out when demand increases

Scales in when demand decreases

**🔹 Auto Scaling Components**

    Component	                                Description
    
    Launch Template                    	Blueprint for new instances (AMI, instance type, key pair, SGs, etc.)
    
    Auto Scaling Group (ASG)	        Group of EC2 instances managed together
    
    Scaling Policies	                Rules to add/remove instances
    
    Cooldown Period	                    Prevents rapid scaling actions

**🔹 Auto Scaling Architecture**

                     ┌────────────────────┐
                     │   CloudWatch Alarm │
                     └─────────┬──────────┘
                               │
                     ┌─────────▼──────────┐
                     │ Auto Scaling Group │
                     └─────────┬──────────┘
                ┌──────────────┴──────────────┐
                │                             │
       ┌────────▼────────┐           ┌────────▼────────┐
       │ EC2 Instance 1  │           │ EC2 Instance 2  │
       └─────────────────┘           └─────────────────┘







