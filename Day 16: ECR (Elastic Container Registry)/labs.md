# 🧪 Day 16: Amazon ECR (Elastic Container Registry) – Hands-on Labs

## 🎯 Lab Objective

By the end of this lab, you’ll be able to:
- Create and manage a private ECR repository
- Authenticate Docker with ECR
- Build, tag, and push Docker images
- Pull and use images from ECR in ECS or locally

---

## 🧰 Prerequisites

✅ AWS CLI installed and configured  
✅ Docker installed and running  
✅ AWS IAM user or role with the following permissions:
- `AmazonEC2ContainerRegistryFullAccess`
- `AmazonECSFullAccess` (for ECS integration)
- `AmazonS3ReadOnlyAccess` (optional for testing)

---

## 🧩 Lab 1: Create an ECR Repository

### Step 1 — Create the repository
```bash
aws ecr create-repository --repository-name myapp --region us-east-1

Expected Output:

{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-1:123456789012:repository/myapp",
        "repositoryUri": "123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp"
    }
}

✅ Repository created successfully!

🧩 Lab 2: Authenticate Docker with ECR

ECR uses temporary tokens for authentication.

Step 1 — Get login password and authenticate:

aws ecr get-login-password --region us-east-1 | \
docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com

If successful, you’ll see:

Login Succeeded

