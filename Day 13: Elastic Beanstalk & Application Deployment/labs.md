# 🧪 Day 13: AWS Elastic Beanstalk – Hands-On Labs

In these labs, you’ll deploy and manage web applications using AWS Elastic Beanstalk — from basic setup to scaling and monitoring.

---

## 🚀 Lab 1: Create and Deploy Your First Web App

### 🎯 Goal
Deploy a simple web application using Elastic Beanstalk (EB) with minimal manual configuration.

### 🧭 Steps
1. Go to **AWS Management Console → Elastic Beanstalk**.  
2. Click **Create Application**.  
3. Enter:
   - Application name: `MyFirstApp`
   - Platform: `Python` (or Node.js, Java, etc.)
   - Sample application: ✅ Use sample code
4. Click **Create environment** → “Web server environment”.
5. Wait a few minutes for AWS to:
   - Create EC2 instance(s)
   - Create S3 bucket
   - Create security group, load balancer, and scaling group
6. Once ready, click the **URL** shown (e.g., `http://myfirstapp-env.eba-xyz.ap-south-1.elasticbeanstalk.com`).

### ✅ Expected Result
You see the Elastic Beanstalk **sample web app page** live and running!

### 💡 Explanation
Elastic Beanstalk automatically created and configured:
- EC2 instance
- Load balancer
- Auto Scaling group
- S3 bucket for deployment
- CloudWatch monitoring

---

## 🧪 Lab 2: Deploy Your Custom Application

### 🎯 Goal
Deploy your own web application ZIP file (e.g., Flask or Node.js app).

### 🧭 Steps
1. Create a simple Flask app (example `app.py`):
   ```python
   from flask import Flask
   app = Flask(__name__)

   @app.route('/')
   def home():
       return "Hello from Flask on Elastic Beanstalk!"

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=8080)


Add a file named requirements.txt:

Hello from Flask on Elastic Beanstalk!

✅ Expected Result

Your custom application is deployed successfully through Elastic Beanstalk.

🧪 Lab 3: Configure Auto Scaling and Load Balancing
🎯 Goal

Enable high availability and automatic scaling.

🧭 Steps

Go to Elastic Beanstalk → Your Environment → Configuration.

Under Capacity → Edit:

Environment type: Load balanced

Instances: Minimum = 1, Maximum = 3

Instance type: t3.micro

Under Load Balancer:

Enable HTTP (port 80)

Enable health checks

Click Apply changes.

💡 Explanation

Beanstalk uses an Auto Scaling Group and an Application Load Balancer (ALB) to distribute load and ensure uptime.

✅ Expected Result

When traffic increases, EB automatically adds EC2 instances to balance load.

🧪 Lab 4: Environment Variables & Configuration Files
🎯 Goal

Set environment variables for application runtime and use .ebextensions.

🧭 Steps

Go to Configuration → Software → Edit.

Add Environment Variables:

ENV=production
DEBUG=False
APP_SECRET=mysecretkey

(Optional) Create a folder in your project:

.ebextensions/options.config

option_settings:
  aws:autoscaling:launchconfiguration:
    InstanceType: t3.micro
  aws:elasticbeanstalk:application:environment:
    LOG_LEVEL: INFO

Re-zip and deploy again.

✅ Expected Result

Your app reads these environment variables from the Beanstalk environment.

🧪 Lab 5: Blue-Green Deployment (Zero Downtime)
🎯 Goal

Deploy a new version of the app safely without downtime.

🧭 Steps

Go to Elastic Beanstalk → Create New Environment.

Choose the same application but different environment name:
e.g., MyFirstApp-Green.

Deploy the new version of the app in the green environment.

Test the new URL.

Once verified, go to Actions → Swap Environment URLs.

💡 Explanation

The new version becomes active (green → production), and the old one (blue) is safely stopped.

✅ Expected Result

Zero downtime app upgrade — users see the new version instantly.

🧪 Lab 6: Integrate with RDS Database
🎯 Goal

Use an RDS instance (e.g., MySQL) inside your Elastic Beanstalk app.

🧭 Steps

Create an RDS instance (MySQL) in the same VPC as your Beanstalk app.

Get the endpoint, username, and password.

In your Flask app:

import pymysql
conn = pymysql.connect(
    host="your-rds-endpoint",
    user="admin",
    password="password",
    database="mydb"
)

Add these credentials as environment variables in Beanstalk:

DB_HOST, DB_USER, DB_PASS, DB_NAME

Re-deploy your app.

✅ Expected Result

Your web app connects to the RDS database securely.

🧪 Lab 7: Monitor and View Logs
🎯 Goal

Use CloudWatch and Elastic Beanstalk logs for debugging.

🧭 Steps

Go to Monitoring → CloudWatch metrics to view:

CPU Utilization

Latency

Request Count

4xx / 5xx errors

Go to Logs → Request Logs → Last 100 lines.

You can also download full logs or configure log streaming to S3.

✅ Expected Result

You can monitor environment performance and troubleshoot easily.

🧪 Lab 8: Clean Up Resources
🎯 Goal

Avoid unwanted charges.

🧭 Steps

Terminate the Elastic Beanstalk environment.

Delete the application version.

Delete related resources (S3 bucket, RDS instance if created).

✅ Expected Result

No resources left running in Elastic Beanstalk.

