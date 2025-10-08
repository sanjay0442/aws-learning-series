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

**🔹 Scaling Policies**

    Type	                                                Description
    
    Target Tracking	            Maintain metric at target value (e.g., 60% CPU)
    
    Step Scaling	            Add/remove instances in steps (e.g., +1 if CPU > 70%)
    
    Simple Scaling	            Add/remove fixed instances based on single threshold
    
    Scheduled Scaling	        Scale at specific times (e.g., business hours)

**🔹 Example Policy**

    If average CPU > 70% for 5 minutes → Add 1 instance.
    
    If CPU < 30% for 10 minutes → Remove 1 instance.

**🧩 Hands-On: Create Auto Scaling Group (ASG)**

    🔧 Steps
    
    Create Launch Template with Amazon Linux + Apache setup.
    
    Create Auto Scaling Group:
    
    Select Launch Template
    
    Choose 2 AZs
    
    Desired: 2, Min: 1, Max: 4
    
    Add Target Group (attach to ALB).
    
    Create Scaling Policy:
    
    Target tracking on average CPU utilization (60%).
    
    Use CloudWatch Alarm to simulate load.

✅ Result: Instances scale in/out automatically.

**🔹 Test Scaling**

Simulate CPU load:

    sudo yum install stress -y
    stress --cpu 2 --timeout 120

Check CloudWatch → Alarms → Auto Scaling Actions.

**🔹 Scheduled Scaling Example**

    Morning (9 AM): scale out to 4 instances
    
    Night (10 PM): scale in to 1 instance

**🧰 ELB + Auto Scaling Integration**

    ELB distributes incoming traffic.
    
    Auto Scaling adds/removes instances automatically.
    
    Health checks ensure only healthy instances serve traffic.
    
    ✅ Combined → Highly available, self-healing architecture

**🧪 Hands-On Summary**

Lab	                    Task	                                    Output

1	                    Create ALB	                                Distribute traffic
2	                    Attach EC2s	                                Load balanced response
3	                    Create ASG	                                Auto-scaling group ready
4	                    Add Scaling Policy	                        Auto scale up/down
5	                    Test Load	                                Scale out under pressure
6	                    Schedule Scaling	                        Predictable resource plan


**🎯 Interview Questions**

    Difference between ALB, NLB, and CLB?
    
    How does Auto Scaling improve fault tolerance?
    
    What are Scaling Policies?
    
    What is Cross-Zone Load Balancing?
    
    Can you attach one ASG to multiple ALBs?
    
    How do you integrate CloudWatch with Auto Scaling?
    
    What happens if an instance in ASG fails?
    
    What are Target Groups?
    
    What is the cooldown period in Auto Scaling?
    
    Explain how ELB and ASG work together.





