# 🐳 Day 16: Amazon ECR (Elastic Container Registry)

## 🧠 What is Amazon ECR?

**Amazon Elastic Container Registry (ECR)** is a **fully managed container image registry** that allows you to **store, manage, and deploy Docker container images** securely and at scale.

Think of it like **GitHub for your Docker images** — you push and pull container images using Docker CLI or the AWS CLI.

---

## ⚙️ How It Works

1. **Build a Docker image** on your local system or CI/CD pipeline.
2. **Authenticate** to your ECR registry using AWS credentials.
3. **Push the image** to the ECR repository.
4. **Pull the image** from ECR when deploying to ECS, EKS, or EC2.

---

## 🧩 Key Components

| Component | Description |
|------------|-------------|
| **Registry** | The overall container image storage (unique per AWS account per region). |
| **Repository** | Logical group of images (like folders). Example: `myapp/backend` |
| **Image** | The actual Docker image (e.g., `v1`, `v2` tags). |
| **Tag** | Version or label assigned to a specific image. |
| **URI** | The unique address of your image (used for pull/push). Example: `123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest` |

---

## 🔐 Authentication

Before pushing or pulling an image, you must **authenticate Docker** to your AWS ECR:

```bash
aws ecr get-login-password --region us-east-1 | \
docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com

This command retrieves a temporary auth token for Docker.

🧰 Core Features

Private & Public Repositories:

Private → For your organization only.

Public → Anyone can pull your images.

Lifecycle Policies:
Automatically clean up old or unused images to save storage costs.

Encryption:
Images are encrypted at rest using AWS KMS.

Image Scanning:
Detect vulnerabilities (CVEs) in your container images.

Integration:
Works seamlessly with ECS, EKS, CodeBuild, CodePipeline, and Lambda.

🧱 Example Workflow

Create a Repository

  aws ecr create-repository --repository-name myapp

Build a Docker Image

  docker build -t myapp .

Tag the Image

  docker tag myapp:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest

Push the Image

  docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest

Pull the Image

  docker pull 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest
/
🧩 ECR Integration with ECS/EKS

In ECS, define the image in your task definition:

{
  "image": "123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest"
}

In EKS, specify it in your Kubernetes Deployment YAML:

containers:
  - name: myapp
    image: 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest

🧠 Common Interview Questions

What is the difference between ECR and Docker Hub?
→ ECR is AWS-managed, secure, and integrated with IAM; Docker Hub is a public registry.

How does ECR ensure image security?
→ Through IAM-based access control, encryption (KMS), and image scanning.

Can you automate cleanup of old images in ECR?
→ Yes, using Lifecycle Policies.

What’s the ECR URI format?
→ <AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/<repository-name>:<tag>

How can ECS/EKS access private ECR images?
→ Through IAM roles assigned to tasks/pods.

✅ Summary

| Feature         | Description                            |
| --------------- | -------------------------------------- |
| **Purpose**     | Store & manage container images        |
| **Access**      | IAM-based authentication               |
| **Integration** | ECS, EKS, Lambda, CodeBuild            |
| **Security**    | Encryption, IAM, scanning              |
| **Automation**  | Lifecycle policies & CI/CD integration |

🧩 Real-World Example

A DevOps engineer builds Docker images for a microservice app.
Instead of pushing to Docker Hub, they use ECR because:

It integrates with ECS for seamless deployment.

Images remain private within AWS.

It supports automated image scanning for vulnerabilities.

