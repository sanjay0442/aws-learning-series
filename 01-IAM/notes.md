🗂 Day 1: IAM (Identity & Access Management)
📝 Theory Notes (Exam-Oriented)

What is IAM?

AWS global service for authentication & authorization.

Free to use.

Controls who can access AWS resources and what they can do.

Key Components:

User → Permanent identity for humans/apps.

Group → Collection of users with same permissions.

Policy → JSON document defining permissions.

Example Actions: s3:GetObject, ec2:StartInstances.

Example Effect: Allow or Deny.

Role → Temporary credentials, mainly used by AWS services (e.g., EC2 → S3).

MFA (Multi-Factor Authentication) → Extra security using OTP device/app.

Root User → Super-admin account; should not be used for daily work.

IAM Best Practices (High Exam Weightage):
✅ Don’t use Root account except billing.
✅ Enable MFA (root + IAM users).
✅ Grant least privilege permissions.
✅ Use roles for AWS services, not access keys.
✅ Rotate credentials regularly.
✅ Apply policies to groups, not individual users.
