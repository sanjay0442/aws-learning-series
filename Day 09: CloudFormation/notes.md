# 🧱 AWS CloudFormation – Infrastructure as Code (IaC)

## 📘 What is AWS CloudFormation?

AWS **CloudFormation** is a service that lets you **automate the creation, modification, and deletion of AWS resources** (like EC2, S3, VPC, RDS, etc.) by using a **template file** written in **YAML or JSON**.

In short:
> CloudFormation = Infrastructure as Code (IaC)

Instead of clicking around in the AWS Console every time you want to create a resource, you write it once in a **CloudFormation template** and **deploy it automatically**.  

This ensures consistency, speed, and zero manual errors.

---

## 🧩 Key Concepts

| Term | Meaning |
|------|----------|
| **Template** | A YAML/JSON file describing what resources you want to create. |
| **Stack** | The actual set of AWS resources created from a template. |
| **Resources** | The AWS services to be created (e.g., EC2, S3). |
| **Parameters** | User inputs (e.g., instance type, key pair). |
| **Outputs** | Information shown after deployment (e.g., public IP). |
| **Mappings** | Key-value pairs (for region-specific or environment-based data). |
| **Conditions** | Logic (e.g., only create a resource in a specific region). |
| **Change Sets** | A preview of what will be changed before you apply an update. |

---

## 🧠 Why Do We Use CloudFormation?

### 1️⃣ Automation
Create servers, databases, VPCs, IAM roles — all automatically.

### 2️⃣ Reproducibility
Use the same template across **Dev / QA / Prod** environments.

### 3️⃣ Version Control
You can **store templates in GitHub** and track changes.

### 4️⃣ Cost Optimization
Easily **delete the stack** to remove all resources and stop billing.

### 5️⃣ Integration
Works with **AWS CLI**, **CodePipeline**, and **CI/CD** workflows.

---

## 🧱 How CloudFormation Works (Step-by-Step)

1. **Write a Template**
   - Define the AWS services (like EC2, S3, IAM).
   - File extension: `.yaml` or `.json`.

2. **Upload Template**
   - Go to AWS → CloudFormation → “Create Stack” → Upload your file.

3. **CloudFormation Engine**
   - Reads the template.
   - Creates resources in the right order.
   - Handles dependencies automatically.

4. **Stack Created**
   - All resources are visible under one logical group (the Stack).

5. **Modify or Delete Stack**
   - CloudFormation updates or removes everything automatically.

---

## 📄 CloudFormation Template Structure

A template typically has these main sections:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: A short note about what this template does
Parameters:      # (Optional) user inputs
Mappings:        # (Optional) region-based mappings
Resources:       # (Required) AWS resources to create
Outputs:         # (Optional) data to show after creation

⚙️ Example – Simple EC2 Instance Template

AWSTemplateFormatVersion: '2010-09-09'
Description: Launch an EC2 instance using CloudFormation

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c94855ba95c71c99
      KeyName: my-keypair
      Tags:
        - Key: Name
          Value: Demo-EC2

💡 This YAML file tells CloudFormation:

  “Create a t2.micro EC2 instance using the provided key pair.”

When you deploy it, CloudFormation handles:

Creating the instance

Managing dependencies

Tracking all in one stack

📊 Important File Types

| File               | Description                                 |
| ------------------ | ------------------------------------------- |
| `.yaml` / `.json`  | Template file for CloudFormation            |
| `.parameters.json` | Stores parameter values for templates       |
| `.outputs`         | Optional — data output after stack creation |


🧠 Real-World Use Cases

✅ Create and manage entire infrastructures (VPC, EC2, RDS, S3, IAM, etc.)
✅ Replicate environments (Dev / Test / Prod) with one click
✅ Automate deployments (used heavily in DevOps pipelines)
✅ Rollback if deployment fails (CloudFormation auto-reverts changes)
✅ Manage infrastructure cost by deleting stacks after testing

🧩 CloudFormation vs Manual Setup

| Manual Setup         | CloudFormation        |
| -------------------- | --------------------- |
| Click-based          | Code-based            |
| Human error possible | Fully automated       |
| Time-consuming       | Fast and repeatable   |
| Hard to track        | Version-controlled    |
| No rollback          | Auto rollback feature |


🧭 CloudFormation Commands (AWS CLI)

# Validate a template
aws cloudformation validate-template --template-body file://template.yaml

# Create a stack
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --parameters ParameterKey=KeyName,ParameterValue=my-keypair

# List stacks
aws cloudformation list-stacks

# Delete a stack
aws cloudformation delete-stack --stack-name my-stack


🧩 Pro Tip

💡 Always validate your template before creating the stack:

aws cloudformation validate-template --template-body file://my-template.yaml


It will catch syntax errors early.

🚀 Next Step

Now that you understand the concept, move to hands-on labs to create:

EC2 Instance

S3 Bucket

Parameterized Templates

VPC Automation

Stack Outputs

👉 Continue to 09-CloudFormation-Labs.md
