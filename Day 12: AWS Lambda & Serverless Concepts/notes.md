# ⚡ Day 12: AWS Lambda & Serverless Concepts (Theory)

## 🧠 What is Serverless?

**Serverless computing** means you don’t have to manage servers.  
Instead, AWS automatically handles provisioning, scaling, and maintaining the infrastructure —  
you focus only on writing code.

### ✅ Key Idea:
You deploy your code → AWS runs it only when triggered → You pay only for the execution time.

---

## 🧩 What is AWS Lambda?

**AWS Lambda** is the core of AWS Serverless architecture.

It lets you run code without provisioning or managing servers.  
You just write your function (in Python, Node.js, Java, etc.), and Lambda executes it automatically  
in response to events such as:

- API Gateway requests  
- S3 uploads  
- DynamoDB updates  
- CloudWatch alarms  
- SNS/SQS messages  

---

## ⚙️ How AWS Lambda Works

1. **Trigger/Event:** Something happens (e.g., file uploaded to S3)
2. **Invoke:** The event triggers a Lambda function
3. **Execution:** AWS automatically runs your function in a container
4. **Scale:** AWS automatically handles scaling
5. **Billing:** You pay only for the number of requests and compute time (measured in milliseconds)

---

## 🧱 Lambda Core Components

| Component | Description |
|------------|--------------|
| **Function** | The actual code you upload and run. |
| **Event Source** | The AWS service or external source that triggers the Lambda. |
| **Handler** | The entry point of your code (e.g., `lambda_function.lambda_handler`). |
| **Execution Role (IAM Role)** | Grants permission for your Lambda to access other AWS resources. |
| **Environment Variables** | Configuration values your function can use during execution. |
| **Layers** | Shared libraries or dependencies you can attach to multiple functions. |
| **Versions & Aliases** | Allow version control and traffic routing between versions. |

---

## 💡 Example: Simple Lambda Function

Here’s a **Python example** of a simple Lambda function:

```python
def lambda_handler(event, context):
    name = event.get('name', 'World')
    return {
        'statusCode': 200,
        'body': f"Hello, {name}! Welcome to AWS Lambda 🚀"
    }

When invoked (e.g., via API Gateway), it returns a JSON response.

🔄 Lambda Invocation Types

| Type                     | Description                       | Example               |
| ------------------------ | --------------------------------- | --------------------- |
| **Synchronous**          | Waits for function response       | API Gateway           |
| **Asynchronous**         | Event sent; no waiting for result | S3, SNS               |
| **Event Source Mapping** | Polls services and invokes Lambda | DynamoDB Streams, SQS |


🪣 Common Use Cases

| Use Case                     | Description                                            |
| ---------------------------- | ------------------------------------------------------ |
| **Serverless API**           | Using API Gateway + Lambda to serve web APIs           |
| **Automation**               | Run code when an S3 file uploads (e.g., resize images) |
| **Data Processing**          | Stream data from Kinesis or DynamoDB                   |
| **Scheduled Tasks**          | CloudWatch event to run Lambda on a schedule           |
| **Chatbots & Notifications** | Integrate with Slack, Telegram, etc.                   |


💰 Pricing Model

You pay for:

Requests: First 1M requests per month are free

Compute Time: Based on GB-seconds used (memory × duration)

Example:

1 GB memory function

Runs for 1 second

Costs ~$0.0000167 per invocation

⚙️ Lambda Configuration Options

| Setting                     | Description                                 |
| --------------------------- | ------------------------------------------- |
| **Memory (128 MB – 10 GB)** | Impacts performance and cost                |
| **Timeout (max 15 min)**    | Defines max runtime                         |
| **Environment Variables**   | Store secrets or configuration              |
| **Concurrency Limit**       | Number of functions running simultaneously  |
| **VPC Access**              | Run Lambda in private VPC subnets if needed |


🧰 Lambda Integrations with AWS Services

| Service            | Integration Type       | Example                     |
| ------------------ | ---------------------- | --------------------------- |
| **S3**             | Event Trigger          | Process image upload        |
| **DynamoDB**       | Stream Processing      | React to DB changes         |
| **API Gateway**    | HTTP Trigger           | Build RESTful APIs          |
| **CloudWatch**     | Scheduler              | Run cron jobs               |
| **SNS/SQS**        | Messaging              | Consume or publish messages |
| **Step Functions** | Workflow Orchestration | Combine multiple Lambdas    |


⚖️ Advantages of Lambda (Why Use It?)

✅ No server management
✅ Automatic scaling
✅ Cost-effective (pay-per-use)
✅ Integrated security with IAM
✅ Event-driven design
✅ Multi-language support
✅ Built-in monitoring via CloudWatch

⚠️ Limitations

⚙️ Max execution time: 15 minutes
📦 Max deployment package size: 250 MB (unzipped, including layers)
🧠 Cold start delay for first invocation
🌐 Limited storage in /tmp (512 MB temporary disk)

🧩 Lambda Architecture Example

A typical Serverless Web App Architecture:

🧩 Lambda Architecture Example

A typical Serverless Web App Architecture:

[User] → [API Gateway] → [Lambda Function] → [DynamoDB]
                      ↓
                 [CloudWatch Logs]

This setup is secure, scalable, and requires no EC2 or servers.

🧠 AWS Lambda Versions & Aliases

Versions → Immutable snapshots of your Lambda code.

Aliases → Named references (e.g., “dev”, “prod”) that point to specific versions.

Benefit: Easy deployment & rollback between environments.

🔒 Security Best Practices

Use least privilege IAM roles.

Never hard-code credentials — use environment variables or AWS Secrets Manager.

Use VPC for private resources.

Enable X-Ray for tracing.

Apply resource-based policies for invocation permissions.

🧩 AWS X-Ray Integration

AWS X-Ray helps visualize requests passing through your Lambda function — useful for debugging, tracing, and identifying performance bottlenecks.

🧱 Summary

| Concept            | Description                                      |
| ------------------ | ------------------------------------------------ |
| **Serverless**     | Run applications without managing servers        |
| **Lambda**         | AWS service that runs code in response to events |
| **Trigger**        | Event source like S3, API Gateway                |
| **Execution Role** | IAM permissions for Lambda                       |
| **Pricing**        | Pay per execution and duration                   |
| **Use Cases**      | APIs, Automation, Data Processing, Cron Jobs     |
| **Limitations**    | Timeout, package size, cold start                |
| **Integrations**   | S3, API Gateway, DynamoDB, CloudWatch, SNS       |

