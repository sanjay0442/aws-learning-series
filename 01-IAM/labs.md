🧪 Lab (Hands-On)

Lab 1: Create IAM User

IAM → Users → Add User → Name: lab-user

Console access + password

Attach AmazonS3ReadOnlyAccess

Login with IAM URL

👉 Result: User can log in but only read S3 buckets.

Lab 2: Create IAM Group & Add User

IAM → Groups → Create Developers

Attach AmazonEC2ReadOnlyAccess

Add lab-user to Developers

👉 Result: User has S3 ReadOnly + EC2 ReadOnly.

Lab 3: Enable MFA (Root)

IAM Dashboard → Security Status → Enable MFA

Use Authenticator app → Scan QR → Enter 2 codes

👉 Result: Root account requires MFA at login.

(Optional Advanced) Lab 4: IAM Role for EC2

IAM → Roles → Create Role → Select EC2

Attach AmazonS3FullAccess

Launch EC2 with this role

Inside EC2 run:** aws s3 ls **

👉 Result: EC2 can access S3 without credentials.
