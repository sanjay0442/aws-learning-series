**🧠 05 – AWS RDS (Relational Database Service)**
📘 1. Theory: What is Amazon RDS?
  Amazon RDS (Relational Database Service) is a fully managed relational database service that simplifies the setup, operation, and scaling of a database in the     cloud.
You don’t have to manually manage hardware, patching, backups, or scaling — AWS does it for you.

**🔹 Supported Database Engines**

RDS supports multiple database engines:

Database Engine   	Description
Amazon Aurora:	   AWS-built database compatible with MySQL and PostgreSQL; highly performant
MySQL:	           Open-source database, widely used
PostgreSQL:	       Advanced open-source relational database
MariaDB:	         Community-driven MySQL fork
Oracle:            Enterprise-grade relational database
SQL Server:	       Microsoft’s relational database engine

**🔹 Key Features of RDS**
Feature	                            Description
Managed Service:	              AWS automates backups, patching, monitoring, and scaling
Multi-AZ Deployment:	          Provides high availability and failover support
Read Replicas:                  Improves performance by serving read requests from replicas
Automated Backups:            	Automatic daily backups & transaction logs retained up to 35 days
Manual Snapshots:              	You can take manual DB snapshots for long-term storage
Security:                       Integrated with IAM, KMS encryption, and VPC network isolation
Monitoring:                     CloudWatch metrics for performance and health
Performance Insights:          	Helps identify database performance bottlenecks

**🔹 RDS Storage Types**
Storage Type	                      Use Case
General Purpose (SSD)	            Balanced price and performance
Provisioned IOPS (SSD)	          High-performance workloads (I/O intensive)
Magnetic (Deprecated)	            For legacy, infrequent access

**🔹 RDS Instance Classes**

RDS instance classes define CPU, memory, and network capacity.
Examples:
    db.t3.micro → for dev/test
    db.m6g.large → general purpose
    db.r6g.xlarge → memory-optimized

**🔹 RDS Backup Types**

Automated Backups – Managed by AWS (point-in-time recovery available)
Manual Snapshots – Created manually, retained until deleted

**🔹 RDS Security**

Runs inside VPC
Controlled via Security Groups
Encryption with AWS KMS
IAM policies for managing DB actions
SSL connections for data-in-transit encryption

**🔹 RDS High Availability**

Single-AZ Deployment → Cheaper but not fault-tolerant
Multi-AZ Deployment → Automatic failover to standby in another AZ

**🔹 RDS Read Replicas**

Asynchronous replication
Used for read-heavy workloads
You can promote a read replica to a standalone DB

**🔹 RDS Maintenance**
AWS automatically applies minor version updates during your chosen maintenance window.

🏗️ 2. RDS Architecture Overview
Here’s what happens when you create an RDS instance:

1. You select:
  DB Engine (MySQL, PostgreSQL, etc.)

  DB instance class (e.g., db.t3.micro)
  
  Storage type and size
  
  VPC/Subnet and Security Group

3. RDS automatically provisions:
  Primary DB instance

  Optional standby instance (Multi-AZ)
  
  Endpoint for connection

4. You can connect from:
  EC2 instance

  AWS Lambda function
  
  On-premises system
  
  Local workstation (if public access enabled)
