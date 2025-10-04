# 🧪 AWS VPC – Hands-On Labs (with Full Explanation)

---

## 🧩 Lab 1: Create a Custom VPC

### 🎯 Goal
Understand how to create an isolated private network in AWS instead of using the default VPC.

### 🧭 Steps
1. Go to **AWS Console → VPC → Your VPCs → Create VPC**
2. Choose **VPC only**
3. Enter:
   - Name: `My-Custom-VPC`
   - IPv4 CIDR block: `10.0.0.0/16`
   - Tenancy: `Default`
4. Click **Create VPC**

### 💡 Explanation
The CIDR block `10.0.0.0/16` means all IPs from `10.0.0.0` → `10.0.255.255` (65,536 IPs).  
You now have your own private virtual network.

### ✅ Expected Result
A new VPC named **My-Custom-VPC** is created and visible in your VPC list with the CIDR `10.0.0.0/16`.

---

## 🧩 Lab 2: Create Public and Private Subnets

### 🎯 Goal
Divide your VPC into **Public** (Internet-accessible) and **Private** (internal) subnets.

### 🧭 Steps
1. Go to **VPC → Subnets → Create subnet**
2. Choose VPC: `My-Custom-VPC`
3. Add the following subnets:

| Subnet Name | AZ | CIDR | Type |
|--------------|----|------|------|
| Public-Subnet | ap-south-1a | 10.0.1.0/24 | Public |
| Private-Subnet | ap-south-1a | 10.0.2.0/24 | Private |

4. Click **Create subnet**

### 💡 Explanation
Each subnet has 256 IPs (minus 5 reserved by AWS).  
You’ll later attach an **Internet Gateway** to the public subnet and a **NAT Gateway** for the private subnet.

### ✅ Expected Result
Two subnets created under your VPC: one **Public**, one **Private**.

---

## 🧩 Lab 3: Attach an Internet Gateway (IGW)

### 🎯 Goal
Enable Internet access for resources in your **Public Subnet**.

### 🧭 Steps
1. Go to **VPC → Internet Gateways → Create Internet Gateway**
   - Name: `My-IGW`
2. Attach the IGW:
   - Actions → Attach to VPC → Select `My-Custom-VPC`
3. Modify the Route Table:
   - Go to **Route Tables → Find your VPC’s Route Table**
   - Add route:  
     - Destination: `0.0.0.0/0`  
     - Target: `Internet Gateway (My-IGW)`
   - Associate this route table with **Public-Subnet** only.

### 💡 Explanation
The route `0.0.0.0/0 → IGW` means:
> “Any traffic not meant for the VPC should go to the Internet Gateway.”

### ✅ Expected Result
Instances in the **Public Subnet** can reach the Internet.

---

## 🧩 Lab 4: Configure NAT Gateway for Private Subnet

### 🎯 Goal
Allow EC2 instances in the **Private Subnet** to access the Internet **outbound only** (for OS updates, etc.).

### 🧭 Steps
1. Create an **Elastic IP**:  
   - EC2 → Network & Security → Elastic IPs → Allocate
2. Go to **VPC → NAT Gateways → Create NAT Gateway**
   - Name: `My-NAT`
   - Subnet: `Public-Subnet`
   - Elastic IP: select the one created above
3. Modify Route Table for **Private Subnet**:
   - Add route:
     - Destination: `0.0.0.0/0`
     - Target: `NAT Gateway (My-NAT)`

### 💡 Explanation
NAT Gateway acts as a **proxy** for private subnets.  
Private EC2s can **access the Internet** but **are not accessible from** it.

### ✅ Expected Result
Private EC2 can run: sudo yum update -y

**without being publicly reachable.**^^^

**🧩 Lab 5: Launch EC2 Instances in Public & Private Subnets**
🎯 Goal

Test communication and access between both subnets.
🧭 Steps
    Launch Public EC2 in Public-Subnet
    Name: Public-EC2
    SG: Allow SSH (port 22) from your IP
    Launch Private EC2 in Private-Subnet
    Name: Private-EC2
    SG: Allow SSH (22) from Public-EC2’s Security Group ID, not 0.0.0.0/0.
    SSH into Public instance:
    ssh -i my-key.pem ec2-user@<public-ec2-ip>
    From inside Public EC2, connect to Private EC2:
    ssh ec2-user@<private-ec2-private-ip>

💡 Explanation

You are using the Public-EC2 as a Bastion Host (Jump Server) to securely reach your private instance.

✅ Expected Result

Private EC2 is accessible only through the Public EC2 — not from the Internet.

**🧩 Lab 6: Create & Test Security Group and Network ACL**
🎯 Goal

Understand the difference between instance-level (SG) and subnet-level (NACL) security.

🧭 Steps
🔸 Security Group (SG)
Add inbound rule → Allow SSH (22) from your IP only
Add inbound HTTP (80) → From anywhere

🔸 Network ACL (NACL)
    Go to VPC → Network ACLs → Select the one for your subnet
    Edit inbound & outbound rules:
    Allow port 22 and 80
    Deny port 3389 (to test blocking)

💡 Explanation
SGs are stateful → reply traffic is automatically allowed.
NACLs are stateless → must explicitly allow both inbound and outbound traffic.

✅ Expected Result

Your EC2 behaves according to the rules — if NACL denies a port, it remains blocked even if SG allows it.

**🧩 Lab 7: Access S3 Using a VPC Endpoint**
🎯 Goal

Allow private EC2 to access S3 without Internet or NAT Gateway.
🧭 Steps
    Go to VPC → Endpoints → Create Endpoint
    Service: com.amazonaws.<region>.s3
    Type: Gateway
    VPC: My-Custom-VPC
    Route table: select Private Subnet route table
    SSH into Private EC2 and test:
    aws s3 ls

💡 Explanation

The S3 VPC Endpoint provides a private route to AWS services.
Data never leaves the AWS internal network → faster and more secure.

✅ Expected Result

Private EC2 lists S3 buckets successfully without Internet access.

**🧩 Lab 8: VPC Peering**
🎯 Goal

Connect two VPCs privately.
🧭 Steps
    Create another VPC → 10.1.0.0/16
    Go to VPC → Peering Connections → Create Peering
    Requester: My-Custom-VPC
    Accepter: New-VPC
    Accept the request from the second VPC.
    Edit route tables in both VPCs:
    Add route → 10.1.0.0/16 ↔ 10.0.0.0/16 via Peering ID

💡 Explanation

Now, resources in both VPCs can communicate privately over the AWS backbone.

✅ Expected Result

Ping or SSH between EC2s in both VPCs using their private IPs.

**🧩 Lab 9: Clean-Up**
🎯 Goal

Avoid charges by deleting unused resources.
🧭 Steps:
    Terminate EC2 instances
    Delete NAT Gateway (to stop hourly charges)
    Delete Subnets, IGW, and VPC

✅ All resources are now deleted, and the environment is clean.

🏁 Summary
    By completing these labs, you have learned:
    How to create a Custom VPC
    Configure Public & Private Subnets
    Use IGW and NAT Gateway
    Implement Security Layers (SG & NACL)
    Connect to S3 using VPC Endpoints
    Establish VPC Peering




