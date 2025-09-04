# Day 1: AWS IAM – Hands-On Labs

## 🧪 Lab 1: Create IAM User
1. Go to **IAM → Users → Add User**
2. Username: `lab-user`
3. Select **Console access** + set custom password
4. Attach policy: `AmazonS3ReadOnlyAccess`
5. Save IAM login URL → Log in as `lab-user`

👉 **Expected Result:** User can log in but only read S3 buckets.

---

## 🧪 Lab 2: Create IAM Group & Add User
1. IAM → Groups → Create group `Developers`
2. Attach `AmazonEC2ReadOnlyAccess`
3. Add `lab-user` to `Developers`

👉 **Expected Result:** User has **S3 ReadOnly + EC2 ReadOnly** access.

---

## 🧪 Lab 3: Enable MFA for Root
1. IAM Dashboard → Security Status → Enable MFA
2. Choose **Virtual MFA App** (Google Authenticator/Authy)
3. Scan QR code → Enter 2 codes → Confirm

👉 **Expected Result:** Root account now requires MFA at login.

---

## (Optional Advanced) Lab 4: IAM Role for EC2
1. IAM → Roles → Create Role → Select **EC2**
2. Attach `AmazonS3FullAccess`
3. Launch EC2 instance → Attach this role
4. Inside EC2, run:
   ```bash
   aws s3 ls
