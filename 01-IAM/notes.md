🗂 Day 1: IAM (Identity & Access Management)
📝 Theory Notes (Exam-Oriented)

# Day 1: AWS IAM (Identity and Access Management) – Notes

## 📌 What is IAM?
- IAM is a **global service** for authentication & authorization in AWS.
- Free to use.
- Controls **who** can access resources and **what** actions they can perform.

---

## 🏗 Key Components
1. **User** → Permanent identity for humans/apps (login credentials or access keys).
2. **Group** → Logical collection of users with the same permissions.
3. **Policy** → JSON document that defines permissions.
   - Example: `s3:GetObject`, `ec2:StartInstances`
   - Effect: `Allow` or `Deny`
4. **Role** → Temporary credentials, often used by AWS services (EC2 → S3).
5. **MFA (Multi-Factor Authentication)** → Extra security using OTP app/device.
6. **Root User** → Super-admin account. Should be used only for billing/critical tasks.

---

## ✅ IAM Best Practices (Exam Focus)
- ❌ Don’t use Root user for daily tasks.
- 🔐 Enable MFA for root + IAM users.
- 🛡 Grant **least privilege** permissions.
- 🔄 Rotate access keys regularly.
- 📦 Apply policies to **groups**, not individual users.
- 🤝 Use roles instead of sharing access keys.

---

## 📚 References
- [IAM Documentation](https://docs.aws.amazon.com/iam/)
- [IAM Getting Started](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html)
- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)


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
