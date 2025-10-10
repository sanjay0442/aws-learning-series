# ⚙️ Day 12: AWS Lambda – Hands-on Labs

This lab section will help you **practically understand** how AWS Lambda works — from creating functions to integrating with other AWS services like S3 and API Gateway.

---

## 🧪 Lab 1: Create a Basic Lambda Function (Hello World)

### 🎯 Goal
Create your first Lambda function to understand its structure and basic configuration.

### 🧭 Steps
1. Open **AWS Management Console → Lambda → Create function**
2. Choose:
   - **Author from scratch**
   - Function name: `HelloWorldLambda`
   - Runtime: `Python 3.9` (you can also use Node.js, Java, etc.)
   - Permissions → Choose or create **new IAM role with basic Lambda permissions**
3. In the code editor, replace the default code with:
   ```python
   def lambda_handler(event, context):
       name = event.get("name", "World")
       return {
           "statusCode": 200,
           "body": f"Hello, {name}! Welcome to AWS Lambda 🚀"
       }

Click Deploy

Click Test → Configure test event

Event name: testEvent

Event JSON:

{
  "name": "Sanjay"
}

Run Test

✅ Expected Result

Lambda returns:

{
  "statusCode": 200,
  "body": "Hello, Sanjay! Welcome to AWS Lambda 🚀"
}


🧪 Lab 2: Trigger Lambda from S3 Upload
🎯 Goal

Automatically trigger your Lambda function whenever a file is uploaded to an S3 bucket.

🧭 Steps

Go to S3 → Create bucket

Name: lambda-s3-demo-bucket

Region: Same as Lambda function region

Go to Lambda → Create function

Name: S3TriggerLambda

Runtime: Python 3.9

Role: Create a new one with S3 read permissions

Add this code:

import json
def lambda_handler(event, context):
    print("Event:", json.dumps(event))
    bucket = event['Records'][0]['s3']['bucket']['name']
    file = event['Records'][0]['s3']['object']['key']
    print(f"File '{file}' uploaded to bucket '{bucket}'")
    return "Success"

Go to S3 → Properties → Event notifications → Create event notification

Event name: UploadTrigger

Event type: All object create events

Destination: Lambda function → Select S3TriggerLambda

Upload any file (e.g., test.txt) to the bucket.

✅ Expected Result

Your Lambda function is triggered and logs appear in CloudWatch Logs with details about the uploaded file.

🧪 Lab 3: Create Serverless API using API Gateway + Lambda
🎯 Goal

Create a REST API that invokes Lambda via API Gateway.

🧭 Steps

Go to Lambda → Create function

Name: APILambdaDemo

Runtime: Python 3.9

Code:

import json

def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from your API! 😎')
    }

Click Deploy

Go to API Gateway → Create API

Choose HTTP API

Click Add integration → Lambda → Select your function (APILambdaDemo)

Click Create

Copy the Invoke URL and open it in your browser.

✅ Expected Result

You’ll see:

Hello from your API! 😎


🧪 Lab 4: Schedule Lambda Function (CloudWatch Event Rule)
🎯 Goal

Trigger Lambda automatically on a schedule (like a cron job).

🧭 Steps

Go to Lambda → Create function

Name: ScheduledLambda

Runtime: Python 3.9

Permissions: Basic Lambda execution role

Code:

import datetime

def lambda_handler(event, context):
    now = datetime.datetime.now()
    print(f"Lambda executed at {now}")
    return "Task executed"


Click Add trigger → EventBridge (CloudWatch Events)

Rule type: Schedule expression

Example: rate(5 minutes)

Deploy and Save.

✅ Expected Result

Lambda runs automatically every 5 minutes.
You can check execution logs in CloudWatch → Logs → Log groups → /aws/lambda/ScheduledLambda

🧪 Lab 5: Store Data in DynamoDB using Lambda
🎯 Goal

Insert records into DynamoDB using Lambda.

🧭 Steps

Go to DynamoDB → Create table

Table name: Users

Partition key: UserID (String)

Create a new Lambda function:

Name: DynamoDBLambda

Runtime: Python 3.9

Role: Attach policy AmazonDynamoDBFullAccess

Add this code:

import boto3
import uuid

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Users')

def lambda_handler(event, context):
    user_id = str(uuid.uuid4())
    table.put_item(Item={
        'UserID': user_id,
        'Name': 'Sanjay',
        'Role': 'DevOps Engineer'
    })
    return {'message': f'User {user_id} added successfully!'}

# ☁️ Day 12: AWS Lambda & Serverless Concepts – Hands-On Labs (Part 2)

These labs continue from the previous Lambda section and focus on **real-world integrations** — DynamoDB, SNS, Step Functions, and Infrastructure-as-Code deployment using AWS CloudFormation.

---

## 🧪 Lab 6: Send Notifications via SNS from Lambda

### 🎯 Goal
Trigger **email/SMS notifications** automatically from Lambda using **Amazon SNS**.

### 🧭 Steps
1. Go to **SNS → Create topic**
   - Type: Standard
   - Name: `LambdaAlertTopic`
2. Under the topic, click **Create subscription**
   - Protocol: Email
   - Endpoint: your email (e.g., `you@example.com`)
3. Confirm the subscription from your inbox.
4. Create a new Lambda function:
   - Name: `SNSNotifierLambda`
   - Runtime: Python 3.9
   - Permissions: Attach policy `AmazonSNSFullAccess`
5. Paste the code below:
   ```python
   import boto3

   sns = boto3.client('sns')
   topic_arn = "arn:aws:sns:REGION:ACCOUNT_ID:LambdaAlertTopic"

   def lambda_handler(event, context):
       message = "🚨 Lambda alert: A new event has been triggered!"
       sns.publish(TopicArn=topic_arn, Message=message)
       return {"status": "Notification sent successfully"}

Replace REGION and ACCOUNT_ID with your values.

Deploy → Test → Run.

✅ Expected Result

You’ll receive an email titled “AWS Notification” with the alert message from your Lambda.

🧪 Lab 7: Step Functions + Lambda (Orchestration)
🎯 Goal

Use AWS Step Functions to coordinate multiple Lambda tasks sequentially.

🧭 Steps

Create three Lambda functions:

StepFuncTask1 → prints “Task 1 started”

StepFuncTask2 → prints “Task 2 started”

StepFuncTask3 → prints “Task 3 completed”

Example code:

def lambda_handler(event, context):
    print("Executing Step Function Task")
    return "Task executed"

Go to AWS Step Functions → Create state machine

Choose Author with code snippets

Choose Standard

Add this definition:

{
  "Comment": "Step Function Example",
  "StartAt": "Task1",
  "States": {
    "Task1": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:StepFuncTask1",
      "Next": "Task2"
    },
    "Task2": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:StepFuncTask2",
      "Next": "Task3"
    },
    "Task3": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:StepFuncTask3",
      "End": true
    }
  }
}


Replace the ARNs accordingly → Click Create state machine.

Click Start execution.

✅ Expected Result

Step Functions executes all 3 Lambda functions in sequence and shows green success flow.

🧪 Lab 8: Deploy Lambda with CloudFormation
🎯 Goal

Automate Lambda deployment using CloudFormation (Infrastructure as Code).

🧭 Steps

Create a new YAML file lambda-template.yaml:

AWSTemplateFormatVersion: '2010-09-09'
Description: Deploy a Lambda Function using CloudFormation

Resources:
  MyLambdaRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          - Effect: Allow
            Principal:
              Service: lambda.amazonaws.com
            Action: sts:AssumeRole
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

  MyLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: CloudFormationLambda
      Handler: index.lambda_handler
      Role: !GetAtt MyLambdaRole.Arn
      Runtime: python3.9
      Code:
        ZipFile: |
          def lambda_handler(event, context):
              print("Hello from CloudFormation Lambda 🚀")
              return "Deployed successfully!"


Save the file and upload it in CloudFormation → Create Stack → With new resources (standard).

Upload your lambda-template.yaml.

Click Next → Next → Create stack.

✅ Expected Result

CloudFormation deploys the Lambda and IAM role automatically.
You can verify under Lambda → CloudFormationLambda function.

🧪 Lab 9: Connect Lambda + S3 + DynamoDB (Mini Project)
🎯 Goal

Build a serverless data pipeline where:

S3 upload triggers Lambda

Lambda parses data and inserts it into DynamoDB

🧭 Steps

Create an S3 bucket: lambda-pipeline-demo

Create DynamoDB table:

Name: FileMetadata

Partition key: FileName (String)

Create Lambda function:

Name: S3toDynamoDB

Role: Add AmazonS3ReadOnlyAccess + AmazonDynamoDBFullAccess

Code:

import boto3
import datetime

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('FileMetadata')

def lambda_handler(event, context):
    for record in event['Records']:
        bucket = record['s3']['bucket']['name']
        file_name = record['s3']['object']['key']
        timestamp = datetime.datetime.now().isoformat()

        table.put_item(Item={
            'FileName': file_name,
            'Bucket': bucket,
            'UploadedAt': timestamp
        })
        print(f"Inserted {file_name} from {bucket}")


Configure S3 → Properties → Event notifications

Event: All object create events

Destination: S3toDynamoDB Lambda

Upload any file into the S3 bucket.

✅ Expected Result

Lambda triggers automatically

DynamoDB table updates with file details:

FileName: test.txt
Bucket: lambda-pipeline-demo
UploadedAt: 2025-10-04T15:32:10


🧪 Lab 10: Monitor Lambda Performance with CloudWatch
🎯 Goal

Analyze metrics, logs, and set alarms for your Lambda functions.

🧭 Steps

Go to CloudWatch → Logs → Log groups → /aws/lambda/YourFunctionName

Observe real-time execution logs.

Go to CloudWatch → Metrics → All metrics → Lambda → By Function Name

Track metrics: Invocations, Errors, Duration, Throttles

Create Alarm:

Metric: Errors

Condition: Greater than 1 for 5 minutes

Action: Send notification via SNS topic

✅ Expected Result

Whenever your Lambda fails, CloudWatch triggers an alarm and sends an email alert through SNS.

🎓 Summary

✅ You learned how to:

Create, trigger, and schedule Lambda functions

Integrate with S3, DynamoDB, API Gateway, and SNS

Automate deployments using CloudFormation

Monitor & alert with CloudWatch


