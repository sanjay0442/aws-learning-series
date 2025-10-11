# 🌱 Day 13: AWS Elastic Beanstalk – Theory Notes

## 📘 What is Elastic Beanstalk?

AWS Elastic Beanstalk is a **Platform-as-a-Service (PaaS)** that lets you deploy and manage applications **without worrying about the underlying infrastructure** (servers, load balancers, scaling groups, etc.).

You just provide your **application code**, and Elastic Beanstalk automatically handles:
- EC2 instance provisioning  
- Load balancing  
- Auto scaling  
- Health monitoring  
- Environment configuration  
- Version management  

💡 **In short:** *You focus on code — AWS Beanstalk takes care of everything else.*

---

## ⚙️ How Elastic Beanstalk Works

1️⃣ **You upload your code**  
   - From the AWS Console, CLI, or Git repository.

2️⃣ **Beanstalk creates an environment**  
   - It launches EC2 instances, sets up security groups, load balancer, auto-scaling, and CloudWatch monitoring.

3️⃣ **AWS manages the lifecycle**  
   - Handles deployment, capacity provisioning, load balancing, scaling, and health checks.

---

## 🧩 Key Components

| Component | Description |
|------------|-------------|
| **Application** | The logical container for your project. |
| **Application Version** | A specific deployable version of your app (e.g., a ZIP or WAR file). |
| **Environment** | A running version of your app (e.g., “dev”, “prod”). |
| **Environment Tier** | Web Server Environment Tier (HTTP requests) or Worker Environment Tier (background jobs). |
| **Platform** | The preconfigured runtime (e.g., Python, Node.js, Java, Docker, .NET, Go). |
| **Configuration Template** | A saved set of environment settings used for consistent deployments. |

---

## 🌍 Environment Types

| Environment | Purpose |
|--------------|----------|
| **Web Server Environment** | Runs a web application accessible via HTTP(S). |
| **Worker Environment** | Processes background tasks from an SQS queue. |

---

## 🧱 Architecture Overview

Elastic Beanstalk orchestrates multiple AWS services for you:

- **EC2 Instances** → Run your application code.  
- **Auto Scaling Group (ASG)** → Scales instances based on traffic.  
- **Elastic Load Balancer (ELB)** → Distributes traffic evenly.  
- **S3** → Stores application versions and logs.  
- **CloudWatch** → Monitors health and performance.  
- **RDS (optional)** → For database-backed applications.  

---

## 🚀 Supported Platforms

- Node.js  
- Python  
- Java (Tomcat)  
- .NET Core on Windows/Linux  
- PHP  
- Ruby  
- Go  
- Docker (custom images)

---

## 📦 Deployment Process

1. Package your app (ZIP, WAR, etc.)  
2. Upload to Elastic Beanstalk  
3. Choose platform and environment  
4. Beanstalk provisions resources  
5. Application deployed automatically  
6. Monitor environment health via dashboard  

---

## 🧰 Deployment Options

| Deployment Type | Description |
|-----------------|-------------|
| **All at Once** | Fastest; deploys to all instances simultaneously (downtime possible). |
| **Rolling** | Deploys in batches; maintains partial availability. |
| **Rolling with Additional Batch** | Launches new instances before updating others (no downtime). |
| **Immutable** | Launches new instances with new version; switches traffic only after healthy (zero-downtime safe deployment). |
| **Blue/Green Deployment** | Separate environments (Blue = current, Green = new). Swap URLs when Green is ready. |

---

## 🛠️ Configuration Management

You can customize environment settings through:
- **Configuration files (.ebextensions)**  
- **Elastic Beanstalk CLI (EB CLI)**  
- **AWS Management Console**

Example `.ebextensions/options.config`:

```yaml
option_settings:
  aws:autoscaling:launchconfiguration:
    InstanceType: t3.micro

🧩 Monitoring & Troubleshooting

Health Dashboard → Shows instance and app health.

CloudWatch Metrics → Monitor CPU, latency, request count.

Logs → View or download via console.

Events Tab → Track environment changes and deployment issues.

💡 Best Practices

✅ Use Immutable or Blue/Green deployments for production.
✅ Enable Enhanced Health Monitoring for real-time status.
✅ Store application logs in S3.
✅ Use .ebextensions for consistent configuration.
✅ Integrate with RDS or ElastiCache for data persistence.
✅ Secure environments using IAM roles and HTTPS.

🧠 Common Interview Questions

What is Elastic Beanstalk?

Difference between Beanstalk and CloudFormation?

Explain the difference between Rolling and Immutable deployment.

How do you monitor Beanstalk environment health?

Can Elastic Beanstalk be used for Docker containers?

What services does Beanstalk use internally?

What is the purpose of the .ebextensions folder?

🧾 Summary

| Feature          | Description                                    |
| ---------------- | ---------------------------------------------- |
| **Service Type** | Platform as a Service (PaaS)                   |
| **Purpose**      | Simplify deployment and management of web apps |
| **Key Benefit**  | Automatic scaling, monitoring, and updates     |
| **Integration**  | EC2, ELB, Auto Scaling, S3, CloudWatch         |
| **Use Case**     | Deploy web apps fast without managing servers  |

  aws:elasticbeanstalk:application:environment:
    ENV: production
