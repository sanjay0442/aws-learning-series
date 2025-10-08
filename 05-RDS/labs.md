🧪 3. RDS Labs (Hands-On Exercises)

Let’s get practical now 💪

🔹 Lab 1: Launch an RDS MySQL Instance

Objective: Deploy a basic MySQL database using AWS Console or CLI.

Steps (Console):

Go to RDS → Databases → Create Database.

Choose Standard create.

Select MySQL engine.

Choose Free tier.

DB instance identifier: rds-mysql-lab.

Username: admin, Password: <your_password>.

Choose VPC (default or custom).

Connectivity: enable Public access (for testing).

Security group: allow inbound MySQL (TCP 3306) from your IP.

Create Database and wait 5–10 minutes.

Verify Connection:

mysql -h <RDS-endpoint> -u admin -p

🔹 Lab 2: Connect from EC2 to RDS

Objective: Connect to RDS from an EC2 instance within the same VPC.

Steps:

Launch an EC2 instance in same VPC.

SSH into EC2:

ssh -i key.pem ec2-user@<public-ip>


Install MySQL client:

sudo yum install mysql -y


Connect:

mysql -h <RDS-endpoint> -u admin -p

🔹 Lab 3: Create Database & Table

Inside MySQL:

CREATE DATABASE testdb;
USE testdb;
CREATE TABLE employees (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(50),
  department VARCHAR(50)
);
INSERT INTO employees (name, department)
VALUES ('John', 'HR'), ('Sita', 'Finance');
SELECT * FROM employees;

🔹 Lab 4: Take Manual Snapshot

Steps:

In RDS console → select DB → Actions → Take snapshot.

Name: rds-mysql-snapshot-1.

Verify it appears under “Snapshots”.

🔹 Lab 5: Restore DB from Snapshot

Steps:

Select the snapshot → Actions → Restore snapshot.

Give new DB name rds-restore-lab.

Launch and test connection.

🔹 Lab 6: Enable Multi-AZ Deployment

Steps:

Modify DB → enable Multi-AZ deployment.

AWS creates standby instance in another AZ.

Test failover by rebooting with failover:

Actions → Reboot → check "Reboot with failover".

🔹 Lab 7: Create Read Replica

Steps:

Select DB → Actions → Create Read Replica.

Set name rds-readreplica-lab.

After creation, connect using replica endpoint.

Test read-only access.

🔹 Lab 8: Configure CloudWatch Alarms

Create CloudWatch alarms for:

CPU Utilization > 80%

Free Storage Space < 1 GB

This helps you monitor DB performance.

🔹 Lab 9: Enable Encryption

When creating a new RDS instance, enable:

Encryption at rest (KMS)

SSL for connections

Verify encryption:

SHOW VARIABLES LIKE 'have_ssl';

🔹 Lab 10: Delete RDS Instance (Clean-up)

Always delete lab instances to avoid charges:

aws rds delete-db-instance --db-instance-identifier rds-mysql-lab --skip-final-snapshot

🧭 Interview / Practice Questions

What’s the difference between Single-AZ and Multi-AZ?

How do Read Replicas work in RDS?

Can you encrypt existing RDS databases?

What are automated backups vs manual snapshots?

How do you connect RDS to EC2?

Explain RDS failover process.

What is the difference between Aurora and RDS MySQL?
