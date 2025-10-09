
---

## 🧪 File 2 → `09-CloudFormation-Labs.md`

```markdown
# 🧪 AWS CloudFormation – Hands-On Labs

Each lab builds on the previous one. You’ll write YAML templates and deploy them via the **AWS Console** or **CLI**.

---

## 🧩 Lab 1: Create a Simple EC2 Instance

### 🎯 Goal:
Launch a basic EC2 instance using CloudFormation.

### 🧾 Template: `ec2-simple.yaml`

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Simple EC2 instance

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c94855ba95c71c99
      KeyName: my-keypair
      Tags:
        - Key: Name
          Value: CFN-EC2

🧭 Steps:

Go to AWS → CloudFormation → Create Stack → Upload ec2-simple.yaml

Click Next → Next → Create Stack

Wait until status shows CREATE_COMPLETE

✅ Result: EC2 instance created automatically.

🧩 Lab 2: Add Parameters (Dynamic Input)
🎯 Goal:

Allow users to choose Instance Type and Key Pair.

🧾 Template: ec2-parameter.yaml

AWSTemplateFormatVersion: '2010-09-09'
Description: EC2 with dynamic parameters

Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    AllowedValues:
      - t2.micro
      - t2.small
      - t2.medium
    Description: Select instance type

  KeyPairName:
    Type: AWS::EC2::KeyPair::KeyName
    Description: EC2 Key Pair name

Resources:
  MyEC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      KeyName: !Ref KeyPairName
      ImageId: ami-0c94855ba95c71c99
      Tags:
        - Key: Name
          Value: Param-EC2

✅ Result: You can deploy the same template in multiple configurations.

🧩 Lab 3: EC2 + S3 Bucket Stack
🎯 Goal:

Deploy multiple resources together.

🧾 Template: ec2-s3.yaml

AWSTemplateFormatVersion: '2010-09-09'
Description: EC2 and S3 bucket via CloudFormation

Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-demo-bucket-12345

  MyEC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c94855ba95c71c99
      KeyName: my-keypair


✅ Result: Both EC2 and S3 are created as one stack.

🧩 Lab 4: VPC + Subnet + IGW
🎯 Goal:

Build a VPC using YAML.

🧾 Template: vpc.yaml

AWSTemplateFormatVersion: 2010-09-09
Description: Create VPC and Subnet

Resources:
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
        - Key: Name
          Value: My-VPC

  MySubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: 10.0.1.0/24
      MapPublicIpOnLaunch: true
      AvailabilityZone: !Select [0, !GetAZs '']
      Tags:
        - Key: Name
          Value: My-Subnet


✅ Result: A VPC and subnet are created automatically.

🧩 Lab 5: Outputs
🎯 Goal:

Show important details like Instance ID or Public IP.

🧾 Template: ec2-output.yaml

AWSTemplateFormatVersion: '2010-09-09'
Description: EC2 with Outputs

Resources:
  MyEC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      KeyName: my-keypair
      ImageId: ami-0c94855ba95c71c99

Outputs:
  InstanceID:
    Value: !Ref MyEC2
    Description: EC2 Instance ID

  PublicIP:
    Value: !GetAtt MyEC2.PublicIp
    Description: EC2 Public IP


✅ Result: Outputs are visible in the “Outputs” tab after stack creation.

🧩 Lab 6: Delete Stack
🧭 Steps:

Select your stack → Click Delete

Confirm deletion

✅ Result: All resources removed automatically — no manual cleanup.

🚀 Challenge

Create a 3-tier architecture (VPC + EC2 + RDS + S3) using a single CloudFormation file.


---

Would you like me to include **extra labs** for:  
- **IAM Role creation via CloudFormation**  
- **CloudFormation nested stacks** (advanced DevOps use case)?  

These are great for automation projects.

🧩 Lab 7: Create an IAM Role and Attach to EC2
🎯 Goal:

Create an IAM Role with S3 access and attach it to an EC2 instance — using CloudFormation.

🧾 Template: ec2-iam-role.yaml

AWSTemplateFormatVersion: '2010-09-09'
Description: Create EC2 instance with IAM Role and S3 access

Resources:
  MyS3AccessRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal:
              Service: ec2.amazonaws.com
            Action: sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/AmazonS3FullAccess
      Tags:
        - Key: Name
          Value: EC2-S3Role

  MyInstanceProfile:
    Type: AWS::IAM::InstanceProfile
    Properties:
      Roles:
        - !Ref MyS3AccessRole

  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c94855ba95c71c99
      KeyName: my-keypair
      IamInstanceProfile: !Ref MyInstanceProfile
      Tags:
        - Key: Name
          Value: EC2-With-IAMRole

💡 Explanation:

IAM Role allows EC2 to access S3 securely (without using access keys).

Instance Profile acts as a “bridge” to attach the role to EC2.

When you SSH into EC2, you can run:

aws s3 ls


and it will list buckets — no credentials required.

✅ Expected Result:

EC2 instance launched successfully.

It can access S3 directly via IAM Role permissions.

🧩 Lab 8: Nested Stacks (Advanced)
🎯 Goal:

Use nested CloudFormation stacks to organize and reuse templates.

💡 Concept:

You can split large infrastructure into modules (VPC, EC2, IAM, etc.).

A Parent Template calls Child Templates as stacks.

🧾 Example Folder Structure:

cloudformation/
├── parent.yaml
├── vpc.yaml
└── ec2.yaml

🔹 vpc.yaml

AWSTemplateFormatVersion: 2010-09-09
Description: Nested Stack - VPC

Resources:
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      Tags:
        - Key: Name
          Value: NestedVPC

Outputs:
  VpcId:
    Value: !Ref MyVPC


🔹 ec2.yaml

AWSTemplateFormatVersion: 2010-09-09
Description: Nested Stack - EC2

Parameters:
  VpcId:
    Type: String

Resources:
  MyEC2:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c94855ba95c71c99
      KeyName: my-keypair
      NetworkInterfaces:
        - AssociatePublicIpAddress: true
          SubnetId: !Select [0, !GetAZs '']


🔹 parent.yaml

AWSTemplateFormatVersion: 2010-09-09
Description: Parent Stack that calls VPC and EC2

Resources:
  VPCStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/mybucket/vpc.yaml

  EC2Stack:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/mybucket/ec2.yaml
      Parameters:
        VpcId: !GetAtt VPCStack.Outputs.VpcId

💡 Explanation:

Each nested stack is uploaded to S3.

The parent.yaml references them using TemplateURL.

This modular approach is common in DevOps and large AWS environments.

✅ Expected Result:

One parent stack creates multiple resources automatically.

Each child stack manages its own part (network, compute, IAM, etc.).

🧩 Lab 9: Update Stack with Change Set
🎯 Goal:

Safely modify existing infrastructure.

🧭 Steps:

Change instance type in template from t2.micro → t2.small.

Run:

aws cloudformation create-change-set \
  --stack-name my-stack \
  --change-set-name UpdateEC2 \
  --template-body file://ec2-parameter.yaml

Review the Change Set in AWS Console.

Execute the change.

✅ Expected Result:

EC2 instance updated without full recreation.

Safer updates with preview.

🧩 Lab 10: Delete All Stacks

To clean up:

aws cloudformation delete-stack --stack-name my-stack


✅ Removes all AWS resources created by CloudFormation — saves cost.

✅ Summary

| Concept        | Key Takeaway                               |
| -------------- | ------------------------------------------ |
| CloudFormation | Infrastructure as Code                     |
| Template       | YAML/JSON file describing resources        |
| Stack          | Deployed infrastructure from template      |
| IAM Role       | Secure way for EC2 to access AWS resources |

👉 Create a 3-Tier Web Application (VPC + EC2 + RDS + S3 + Load Balancer) using CloudFormation automation —
| Nested Stack   | Modular & reusable infrastructure          |
| Change Set     | Preview updates before applying            |
| Delete Stack   | One-click cleanup                          |


