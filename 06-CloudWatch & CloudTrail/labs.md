**🧪 Hands-On Labs**

🧩 Lab 1: View EC2 Metrics in CloudWatch

  Launch EC2 → Go to CloudWatch → Metrics → EC2
  
  Observe metrics like CPUUtilization, NetworkIn
  
  Add to dashboard
  
  ✅ Result: Live monitoring of EC2 performance.

**🧩 Lab 2: Create CloudWatch Alarm for CPU > 70%**

  CloudWatch → Alarms → Create Alarm
  
  Select metric → EC2 → CPUUtilization
  
  Set threshold 70%, duration 2 data points
  
  Create SNS topic → Add email → Confirm subscription

Test with:

  sudo yum install stress -y
  
  stress --cpu 2 --timeout 60


✅ Result: Email alert when CPU crosses 70%.

**🧩 Lab 3: Configure CloudWatch Logs for EC2**

  Install CloudWatch agent
  
  Run config wizard & start service
  
  Verify logs in CloudWatch → Logs → Log Groups
  
  ✅ Result: EC2 system logs appear in CloudWatch.

**🧩 Lab 4: Automate EC2 Stop at Night**
  
  CloudWatch → Events → Create Rule
  
  Schedule expression:
  
  cron(30 16 * * ? *)  # 10 PM IST
  
  Target: EC2 → StopInstances

✅ Result: EC2 stops automatically at 10 PM.

**🧩 Lab 5: Enable CloudTrail**

  CloudTrail → Trails → Create Trail
  
  Name: My-CloudTrail-Lab
  
  Select “All Regions”
  
  Store logs in new S3 bucket

✅ Result: All AWS activity logged to S3.

**🧩 Lab 6: Verify CloudTrail Logs**

  Go to your S3 bucket
  
  Download and open log file

✅ Result: View all user actions and IP addresses.

**🧩 Lab 7: Enable CloudTrail Insights**

  CloudTrail → Insights → Enable
  
  Wait for data → Observe unusual API spikes

✅ Result: Anomalous behavior detected automatically.

**🧩 Lab 8: Integrate CloudTrail with CloudWatch Logs**

  In CloudTrail → Enable “Send to CloudWatch Logs”
  
  Create Log Group: /aws/cloudtrail/MyTrail
  
  Add Metric Filter for sensitive actions (e.g. DeleteBucket)
  
  Create Alarm → Trigger SNS notification

✅ Result: Instant alert on sensitive actions.

**🧩 Lab 9: Create a CloudWatch Dashboard**

  CloudWatch → Dashboards → Create “My-Monitoring”
  
  Add widgets:
  
  EC2 CPU
  
  RDS Storage
  
  Lambda Errors

✅ Result: Centralized AWS monitoring view.

**🧩 Lab 10: Clean Up**

  aws cloudwatch delete-alarms --alarm-names "HighCPUAlarm"
  
  aws cloudtrail delete-trail --name My-CloudTrail-Lab


✅ Result: No leftover resources or charges.

**🎯 Interview & Real-World Questions**

  Difference between CloudWatch and CloudTrail?
  
  What are the different CloudWatch components?
  
  Can CloudWatch monitor on-prem servers?
  
  What is CloudTrail Insights?
  
  How do you automate EC2 stop/start schedules?
  
  What’s a CloudWatch metric filter?
  
  How can you detect unauthorized access using CloudTrail?
  
  How do CloudWatch alarms integrate with SNS?
  
  What happens if CloudTrail is disabled?
  
  Can CloudWatch and CloudTrail work together?
