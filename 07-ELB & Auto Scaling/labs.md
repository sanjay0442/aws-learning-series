**🧩 Hands-On: Create Auto Scaling Group (ASG)**

  🔧 Steps
  
  Create Launch Template with Amazon Linux + Apache setup.
  
  Create Auto Scaling Group:
  
  Select Launch Template
  
  Choose 2 AZs
  
  Desired: 2, Min: 1, Max: 4
  
  Add Target Group (attach to ALB).
  
  Create Scaling Policy:
  
  Target tracking on average CPU utilization (60%).
  
  Use CloudWatch Alarm to simulate load.

✅ Result: Instances scale in/out automatically.

**🔹 Test Scaling**

Simulate CPU load:

  sudo yum install stress -y
  stress --cpu 2 --timeout 120

Check CloudWatch → Alarms → Auto Scaling Actions.

**🔹 Scheduled Scaling Example**

  Morning (9 AM): scale out to 4 instances
  
  Night (10 PM): scale in to 1 instance

**🧰 ELB + Auto Scaling Integration**

  ELB distributes incoming traffic.
  
  Auto Scaling adds/removes instances automatically.
  
  Health checks ensure only healthy instances serve traffic.
  
  ✅ Combined → Highly available, self-healing architecture

**🧪 Hands-On Summary**

Lab	                    Task	                                    Output

1	                    Create ALB	                                Distribute traffic
2	                    Attach EC2s	                                Load balanced response
3	                    Create ASG	                                Auto-scaling group ready
4	                    Add Scaling Policy	                        Auto scale up/down
5	                    Test Load	                                Scale out under pressure
6	                    Schedule Scaling	                        Predictable resource plan

