🧪 AWS ECS – Hands-On Labs (with Full Explanation)
🎯 Goal

Learn to deploy a containerized web application on AWS ECS using ECR, Fargate, and ALB (Application Load Balancer).

🧭 Lab 1: Create an ECR Repository (Elastic Container Registry)
Steps:

Go to AWS Console → ECR (Elastic Container Registry)

Click Create Repository

Name: mywebapp

Visibility: Private

Click Create Repository

After creation, copy the repository URI, e.g.

123456789012.dkr.ecr.ap-south-1.amazonaws.com/mywebapp

On your local machine (or Cloud9 terminal), authenticate Docker to ECR:

aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.ap-south-1.amazonaws.com

Build and push Docker image:

docker build -t mywebapp .
docker tag mywebapp:latest 123456789012.dkr.ecr.ap-south-1.amazonaws.com/mywebapp:latest
docker push 123456789012.dkr.ecr.ap-south-1.amazonaws.com/mywebapp:latest

💡 Explanation:

You’ve now stored your Docker image in AWS ECR — a secure, version-controlled image registry integrated with ECS.

✅ Expected Result:

Your image is listed under ECR → “mywebapp” → Images tab.

🧭 Lab 2: Create a Task Definition
Steps:

Go to ECS Console → Task Definitions → Create New Task Definition

Choose Fargate launch type.

Configure:

Task name: mywebapp-task

Task role: Create a new IAM role with ECS permissions

CPU: 256

Memory: 512

Add Container:

Name: myweb-container

Image: 123456789012.dkr.ecr.ap-south-1.amazonaws.com/mywebapp:latest

Port mappings: 80

Click Create

💡 Explanation:

A task definition acts like a recipe — defining how containers are launched, how much compute/memory they need, and which ports to open.

✅ Expected Result:

Task Definition mywebapp-task:1 is now visible in ECS.

🧭 Lab 3: Create an ECS Cluster
Steps:

Go to ECS → Clusters → Create Cluster

Choose Fargate.

Enter:

Name: myweb-cluster

VPC: Select existing or create new

Subnets: Choose 2 public subnets in different AZs

Click Create

💡 Explanation:

The cluster is a logical group of resources (in Fargate mode, AWS manages the servers for you).

✅ Expected Result:

Cluster myweb-cluster appears under ECS → Clusters.

🧭 Lab 4: Create an ECS Service
Steps:

Inside myweb-cluster → Click Create Service

Select:

Launch type: Fargate

Task Definition: mywebapp-task

Service name: webapp-service

Number of tasks: 2

Networking:

Select your VPC

Choose public subnets

Assign a security group (allow inbound HTTP 80)

Enable Auto-assign public IP: ENABLED

Load Balancing:

Choose Application Load Balancer

Create a new one named webapp-alb

Listener: HTTP (80)

Target group: webapp-tg (select IP target type)

Click Create Service

💡 Explanation:

The service ensures your app always runs the desired number of tasks and distributes traffic evenly using ALB.

✅ Expected Result:

You see two running tasks under the service, with Running status.

🧭 Lab 5: Verify Your Application
Steps:

Go to EC2 → Load Balancers → webapp-alb

Copy the DNS name (e.g. webapp-alb-123456.ap-south-1.elb.amazonaws.com)

Paste it into your browser → Your containerized web app should load.

💡 Explanation:

ALB forwards traffic to running containers across multiple AZs for high availability.

✅ Expected Result:

App accessible via the ALB DNS name 🎉

🧭 Lab 6: Configure Auto Scaling
Steps:

Go to ECS → myweb-cluster → Services → webapp-service → Auto Scaling tab

Enable Service Auto Scaling

Target Tracking Policy:

Metric type: ECSServiceAverageCPUUtilization

Target value: 50%

Min tasks: 2

Max tasks: 5

Click Save

💡 Explanation:

Now ECS automatically scales the number of containers based on CPU usage — reducing cost and maintaining performance.

✅ Expected Result:

ECS adds/removes tasks automatically when load changes.

🧭 Lab 7: View Logs and Metrics in CloudWatch
Steps:

Go to CloudWatch → Logs → Log Groups

Find /ecs/mywebapp-task

Check application logs.

Also, go to Metrics → ECS → Cluster Metrics to view CPU/Memory graphs.

💡 Explanation:

ECS integrates natively with CloudWatch for centralized monitoring and alerting.

✅ Expected Result:

You can view real-time logs and performance of your ECS containers.

🧭 Lab 8: Clean-Up Resources
Steps:

Delete ECS Service and Cluster.

Delete Load Balancer and Target Group.

Delete ECR repository (optional).

Delete VPC or subnets if created manually.

✅ Expected Result:

All ECS-related resources are cleaned up to avoid unnecessary billing.

🧠 Summary

| Component       | Purpose                                  |
| --------------- | ---------------------------------------- |
| **ECR**         | Stores Docker images                     |
| **ECS Task**    | Defines how a container runs             |
| **ECS Cluster** | Logical grouping for running tasks       |
| **Service**     | Ensures desired tasks are always running |
| **ALB**         | Balances traffic across tasks            |
| **CloudWatch**  | Monitoring & logging                     |
| **Fargate**     | Serverless compute for containers        |
