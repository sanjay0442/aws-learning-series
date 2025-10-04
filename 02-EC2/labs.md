## Lab 1: Launch EC2 Instance (Basic)

### Steps:
1. Go to **EC2 → Launch Instance**
2. Choose **AMI:** Amazon Linux 2
3. Choose **Instance Type:** t2.micro (Free Tier)
4. Configure Key Pair → Create New or Use Existing
5. Configure Network → Select default VPC/Subnet
6. Configure Security Group:
   - Allow SSH (22)
7. Launch and connect via SSH:
   ```bash
   ssh -i my-key.pem ec2-user@<Public_IP>
✅ Result: You can log in to EC2 and execute commands.

Lab 2: Add IAM Role to EC2
Steps:
IAM → Roles → Create Role → Choose EC2
Attach Policy: AmazonS3FullAccess
Name Role: EC2-S3-Access

Launch EC2 Instance and attach this role

Inside EC2:
aws s3 ls
✅ Result: EC2 can access S3 without credentials.

Lab 3: Create and Attach EBS Volume
Steps:
EC2 → Volumes → Create Volume (8GB, same AZ)
Attach Volume to Instance

Connect to EC2 and run:
lsblk
sudo mkfs -t xfs /dev/xvdf
sudo mkdir /data
sudo mount /dev/xvdf /data
df -h
✅ Result: New storage mounted successfully.

Lab 4: Create Custom AMI
Steps:
Stop EC2 → Actions → Image → Create Image
Name: MyWebServer-AMI
Launch new instance from this AMI.

✅ Result: New instance boots with same configuration.

Lab 5: Setup CloudWatch Alarm
Steps:
CloudWatch → Alarms → Create Alarm
Metric: EC2 → CPUUtilization
Threshold: > 70%
Action: Send SNS notification

✅ Result: Alert triggered when CPU exceeds threshold.

Lab 6: Connect EC2 with AWS CLI Profile
Steps:
Configure AWS CLI:

aws configure --profile ec2-admin
Add instance control:

aws ec2 start-instances --instance-ids i-0abcd1234 --profile ec2-admin
✅ Result: EC2 managed via custom CLI profile.

Lab 7: EC2 User Data Script
Steps:
Launch EC2 → Advanced Details → Add User Data:

#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Welcome to AWS EC2" > /var/www/html/index.html
Launch Instance → Access via Public IP.

✅ Result: Apache web page auto-configured at launch.
