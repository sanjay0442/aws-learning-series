# EC2 (Elastic Compute Cloud) - Notes

## 1. What is EC2?

Amazon EC2 (Elastic Compute Cloud) provides resizable compute capacity in the cloud.
It allows you to run virtual machines (called instances) on AWS.

### Key Features:
- On-demand virtual servers
- Variety of instance types (CPU, memory, storage optimized)
- Pay-as-you-go pricing
- Elasticity — scale up/down easily
- Integration with EBS, IAM, VPC, CloudWatch, etc.

---

## 2. EC2 Instance Lifecycle

| State | Description |
|--------|--------------|
| **Pending** | Instance is being created |
| **Running** | Instance is active and usable |
| **Stopping** | Instance is shutting down |
| **Stopped** | Instance stopped (data on EBS persists) |
| **Terminated** | Instance deleted (EBS deleted if not preserved) |

---

## 3. EC2 Components

| Component | Description |
|------------|-------------|
| **AMI (Amazon Machine Image)** | Template for EC2 (OS + Software) |
| **Instance Type** | Defines CPU, memory, storage, network capacity |
| **EBS Volume** | Persistent block storage for EC2 |
| **Security Group** | Virtual firewall for instance traffic |
| **Key Pair** | SSH/RDP login credentials |
| **Elastic IP** | Static public IP for EC2 |
| **User Data** | Script that runs at instance boot |
| **IAM Role** | Provides permissions to EC2 without credentials |

---

## 4. Types of EC2 Instances

| Type | Use Case |
|------|-----------|
| **General Purpose (t3, t4g, m5)** | Balanced CPU/memory |
| **Compute Optimized (c5, c6g)** | High-performance compute |
| **Memory Optimized (r5, x1e)** | In-memory databases |
| **Storage Optimized (i3, d2)** | High disk throughput |
| **Accelerated Computing (p3, g4)** | GPU, ML workloads |

---

## 5. EC2 Pricing Models

| Model | Description |
|--------|-------------|
| **On-Demand** | Pay per hour/second, no commitment |
| **Reserved** | 1–3 year commitment, lower cost |
| **Spot** | Bid for unused capacity, up to 90% cheaper |
| **Savings Plan** | Commit to usage for savings |
| **Dedicated Host** | Physical server dedicated to you |

---

## 6. Storage Options

| Type | Description |
|-------|-------------|
| **EBS (Elastic Block Store)** | Persistent block storage |
| **Instance Store** | Temporary local storage (deleted on stop) |
| **EFS (Elastic File System)** | Shared NFS for Linux instances |
| **S3** | Object storage, external to EC2 |

---

## 7. Security Best Practices

- Use **Key Pairs** (SSH/RDP) for authentication  
- Use **Security Groups** (allow minimal ports)  
- Attach **IAM Role** instead of embedding keys  
- Enable **CloudWatch Alarms** for monitoring  
- Use **EC2 Instance Connect** for safer SSH access  
- Apply **Patching** and use **SSM Agent**

---

## 8. EC2 CLI Commands (Examples)

```bash
# List instances
aws ec2 describe-instances

# Launch instance
aws ec2 run-instances --image-id ami-0abcd1234 --instance-type t2.micro --key-name my-key --security-group-ids sg-12345 --subnet-id subnet-6789

# Stop instance
aws ec2 stop-instances --instance-ids i-0123456789

# Start instance
aws ec2 start-instances --instance-ids i-0123456789

# Terminate instance
aws ec2 terminate-instances --instance-ids i-0123456789

9. Monitoring
      CloudWatch Metrics: CPUUtilization, Disk I/O, Network I/O
      CloudWatch Alarms: Notify or take action on thresholds
      CloudTrail: Track API activity and user actions

10. Common Interview Questions
      What is the difference between EBS and Instance Store?
      How does Security Group differ from NACL?
      What are Spot Instances used for?
      How to make EC2 auto-start on reboot failure?
      How can EC2 access S3 securely?
      What happens when you stop a Spot Instance?
      What are the limits of Elastic IPs?
