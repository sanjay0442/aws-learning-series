# 🧠 Day 3: VPC (Virtual Private Cloud) – Theory Notes

## 1️⃣ What is a VPC?
A **Virtual Private Cloud (VPC)** is your own isolated network within AWS.  
It allows you to define your own IP range, subnets, routing, and security — just like a traditional on-premise data center but software-defined.

Think of it as your **private section of the AWS network**, where you can launch EC2 instances, databases, and other resources securely.

---

## 2️⃣ Key Components of a VPC

| Component | Description |
|------------|-------------|
| **VPC** | The main virtual network container (Example: `10.0.0.0/16`). |
| **Subnets** | Divide your VPC into smaller networks (Public/Private). |
| **Route Table** | Defines where network traffic goes (e.g., to the Internet Gateway, NAT Gateway, or another subnet). |
| **Internet Gateway (IGW)** | Allows public subnets to communicate with the Internet. |
| **NAT Gateway** | Enables private subnets to access the Internet (for updates/downloads) without being exposed. |
| **Security Group (SG)** | Instance-level firewall that controls inbound/outbound traffic (stateful). |
| **Network ACL (NACL)** | Subnet-level firewall that controls traffic to/from subnets (stateless). |
| **Elastic IP (EIP)** | Static public IP address that can be attached to EC2 instances. |
| **Peering Connection** | Connects two VPCs privately using AWS backbone. |
| **VPC Endpoint** | Private connection to AWS services like S3 or DynamoDB, bypassing the Internet. |

---

## 3️⃣ Public vs Private Subnets

| Subnet Type | Internet Access | Example Use Case |
|--------------|----------------|------------------|
| **Public Subnet** | Yes (via Internet Gateway) | Web servers |
| **Private Subnet** | No direct Internet (only via NAT Gateway) | Databases, internal servers |

---

## 4️⃣ VPC CIDR & IP Addressing

When creating a VPC, you define a **CIDR block (Classless Inter-Domain Routing)**.

Example:  
**10.0.0.0/16**


- Total IPs: 65,536  
- Example subnet divisions:
  - `10.0.1.0/24` → Public subnet  
  - `10.0.2.0/24` → Private subnet  

⚠️ AWS reserves **5 IPs** in each subnet (first 4 and last 1).

---

## 5️⃣ Security Layers

| Layer | Function | Stateful/Stateless |
|--------|-----------|--------------------|
| **Security Group (SG)** | Attached to instances | Stateful |
| **Network ACL (NACL)** | Attached to subnets | Stateless |

> **Stateful:** If inbound is allowed, outbound is automatically allowed.  
> **Stateless:** Must explicitly allow both inbound and outbound directions.

---

## 6️⃣ Real-World Analogy

| AWS Concept | Real-World Analogy |
|--------------|--------------------|
| **VPC** | Your office floor |
| **Subnets** | Rooms on that floor |
| **Internet Gateway** | Main building Wi-Fi router |
| **NAT Gateway** | Proxy for internal rooms |
| **Security Group** | Door locks on rooms |
| **NACL** | Gate security at floor entrance |

---

## 7️⃣ Common Interview Questions

1. What’s the difference between a Security Group and a Network ACL?  
2. How do public and private subnets differ?  
3. Can one VPC communicate with another? → Yes, via **VPC Peering** or **Transit Gateway**.  
4. How can private subnets access S3 without the Internet? → Use a **VPC Endpoint**.  
5. What are the default limits? → 5 VPCs per region (can request increase).

---

## 8️⃣ Default VPC

When you create a new AWS account, a **default VPC** exists in each region:
- 1 subnet per Availability Zone
- Internet Gateway attached
- Route table configured  
✅ You can immediately launch EC2 instances without setup.

---

## 9️⃣ VPC Peering

- Connects **two VPCs** privately using AWS backbone.  
- Can be in **same or different AWS accounts**.  
⚠️ No **transitive routing** (A ↔ B ↔ C ≠ A ↔ C).

---

## 🔟 VPC Endpoints

Private connectivity to AWS services without using public Internet.  
Types:
- **Interface Endpoint (PrivateLink)**
- **Gateway Endpoint** (for S3, DynamoDB)

---

## ✅ Summary

| Concept | Description |
|----------|-------------|
| **VPC** | Your private AWS network |
| **Subnets** | Logical network divisions |
| **IGW** | Public Internet access |
| **NAT** | Outbound Internet for private subnets |
| **SG** | Instance-level firewall (stateful) |
| **NACL** | Subnet-level firewall (stateless) |
| **VPC Endpoint** | Private AWS service access |
| **Peering** | Private VPC-to-VPC connection |

---

### 📘 Next Step
Move on to **`Day-3-VPC/labs.md`** for practical hands-on labs:
- Create custom VPC  
- Configure public/private subnets  
- Attach Internet/NAT Gateways  
- Launch EC2 instances in subnets  
- Test connectivity and VPC Endpoints
- Access S3 privately using a VPC Endpoint

