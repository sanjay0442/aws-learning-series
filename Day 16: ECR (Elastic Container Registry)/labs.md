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

🧩 Lab 3: Build a Docker Image
Step 1 — Create a sample Dockerfile

echo 'FROM alpine
CMD echo "Hello from ECR Docker Image!"' > Dockerfile

Step 2 — Build the image

docker build -t myapp .

Step 3 — Verify the image

docker images

You should see:

REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
myapp        latest    a1b2c3d4e5f6   1 minute ago     5MB

🧩 Lab 4: Tag the Docker Image
Step 1 — Tag image with the ECR repo URI

docker tag myapp:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest

Step 2 — Verify the tag

docker images

You’ll now see:

REPOSITORY                                            TAG       IMAGE ID       CREATED          SIZE
123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp    latest    a1b2c3d4e5f6   2 minutes ago    5MB

🧩 Lab 5: Push Image to ECR
Step 1 — Push the image

docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest

Expected output:

The push refers to repository [123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp]
latest: digest: sha256:xxxxxxxxxxxxxxxxxxxxxxxxx size: 1234

✅ Image is now stored in ECR!

🧩 Lab 6: Pull Image from ECR

You can now pull your image from anywhere (EC2, ECS, EKS, or local).

docker pull 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest

Then run it:

docker run myapp

Output:

Hello from ECR Docker Image!

🧩 Lab 7: Enable Image Scanning

Enable automated vulnerability scanning for your repository.

aws ecr put-image-scanning-configuration \
    --repository-name myapp \
    --image-scanning-configuration scanOnPush=true

Now, whenever you push an image, AWS automatically scans it for known CVEs.

🧩 Lab 8: Apply Lifecycle Policy

Automatically remove older images to save space and cost.

Step 1 — Create a lifecycle policy file lifecycle-policy.json

{
  "rules": [
    {
      "rulePriority": 1,
      "description": "Keep last 5 images",
      "selection": {
        "tagStatus": "any",
        "countType": "imageCountMoreThan",
        "countNumber": 5
      },
      "action": {
        "type": "expire"
      }
    }
  ]
}

Step 2 — Apply the policy

aws ecr put-lifecycle-policy \
  --repository-name myapp \
  --lifecycle-policy-text file://lifecycle-policy.json

✅ This will automatically clean up old images beyond the last 5 pushed.

🧩 Lab 9 (Optional): Use ECR Image in ECS Task Definition
Step 1 — Edit your ECS Task Definition JSON

{
  "containerDefinitions": [
    {
      "name": "myapp-container",
      "image": "123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp:latest",
      "essential": true,
      "memory": 256,
      "cpu": 128
    }
  ],
  "family": "myapp-task"
}

Step 2 — Register it

aws ecs register-task-definition \
    --cli-input-json file://task-definition.json

✅ ECS will now pull your Docker image directly from ECR.

🧹 Cleanup Resources

To avoid unnecessary charges, delete the repository after the lab.

aws ecr delete-repository --repository-name myapp --force

🧠 Summary

| Task         | Command                       |
| ------------ | ----------------------------- |
| Create repo  | `aws ecr create-repository`   |
| Login to ECR | `aws ecr get-login-password`  |
| Build image  | `docker build -t myapp .`     |
| Tag image    | `docker tag myapp <repo-URI>` |
| Push image   | `docker push <repo-URI>`      |
| Pull image   | `docker pull <repo-URI>`      |
