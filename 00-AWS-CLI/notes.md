1. What is AWS CLI?

AWS CLI (Command Line Interface) is an open-source tool that lets you interact with AWS services using commands in your terminal.

Instead of clicking in the AWS Management Console, you can automate and script tasks using the CLI.

Official docs: AWS CLI Documentation

2. Why Use AWS CLI?

✅ Faster than console for repetitive tasks

✅ Automation with scripts and DevOps tools

✅ Multi-account support (profiles for dev/test/prod)

✅ Works with IAM roles, MFA, and temporary credentials

✅ Used in CI/CD pipelines (Jenkins, GitHub Actions, GitLab, etc.)

3. AWS CLI Installation
Linux/macOS
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

Windows

Download installer from: AWS CLI Download

Install → Open Command Prompt → run:

aws --version

Verify Installation
aws --version

4. AWS CLI Configuration

Configure with IAM user credentials:

aws configure


It asks for:

AWS Access Key ID

AWS Secret Access Key

Default region (e.g., ap-south-1)

Default output format (json/table/text)

Where credentials are stored:

~/.aws/credentials → Access/Secret keys

~/.aws/config → Region, output, profile info

5. CLI Command Structure

General format:

aws <service> <operation> --options


Examples:

aws s3 ls                    # List S3 buckets
aws ec2 describe-instances   # List EC2 instances
aws iam list-users           # List IAM users

6. Identity Verification

Check who you are logged in as:

aws sts get-caller-identity


Sample output:

{
  "UserId": "AIDAEXAMPLE12345",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/lab-user"
}

7. Multiple Profiles

Useful to manage multiple AWS accounts (dev/test/prod).

Configure multiple profiles:

aws configure --profile dev
aws configure --profile prod


Use profiles:

aws s3 ls --profile dev
aws sts get-caller-identity --profile prod


Example ~/.aws/credentials:

[default]
aws_access_key_id=XXXX
aws_secret_access_key=YYYY

[dev]
aws_access_key_id=AAAA
aws_secret_access_key=BBBB

[prod]
aws_access_key_id=CCCC
aws_secret_access_key=DDDD

8. Where AWS CLI is Used in Industry

Infrastructure automation with shell scripts

CI/CD pipelines (Jenkins, GitHub Actions, GitLab CI/CD)

**🧠 AWS IAM Pricing Overview**

💰 IAM (Identity and Access Management) is completely free —
There are no direct charges for:

Creating IAM users, groups, or roles

Attaching policies (AWS managed or custom)

Using MFA (multi-factor authentication)

Managing permissions or credentials

Accessing the IAM console or API

So — you can create as many IAM users, roles, and policies as you need without worrying about extra IAM costs.

**⚠️ However — there are indirect costs in a few cases:**
| **Feature / Service**                        | **Pricing Impact**                  | **Explanation**                                                                                                                                                                                                |
| -------------------------------------------- | ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **IAM Access Analyzer**                      | 🟡 *Free for findings*              | Basic analyzer (for resource sharing) is free, but if you enable **AWS Organizations access analyzer** (organization-level scanning), that may use **AWS CloudTrail and Config**, which can incur small costs. |
| **IAM Roles with AWS Services**              | 🟢 Free                             | Example: An EC2 instance assumes a role — no cost for the role usage.                                                                                                                                          |
| **Temporary credentials (STS)**              | 🟢 Free                             | Security Token Service (STS) is free to use.                                                                                                                                                                   |
| **AWS Single Sign-On / IAM Identity Center** | 🟢 Free (for identity federation)   | IAM Identity Center itself is free; costs may occur if linked to directory services.                                                                                                                           |
| **CloudTrail logs of IAM activity**          | 🔵 *Charged for CloudTrail storage* | IAM uses CloudTrail for logging. While IAM is free, storing and analyzing logs in S3 or CloudWatch costs money.                                                                                                |
| **MFA (Multi-Factor Authentication)**        | 🟢 Free (virtual MFA)               | Virtual MFA via authenticator apps is free. Physical MFA devices (like hardware tokens) cost extra if purchased.                                                                                               |

**💡 Example Scenarios**

Creating users and roles:
You can create 100 users and 50 roles → still $0.00 for IAM itself.

Logging IAM events:
If CloudTrail logs those events to S3, then S3 storage and CloudTrail data events cost a small amount.

Using IAM with EC2:
An EC2 instance assuming an IAM Role → Free.

Managing multiple AWS accounts

Troubleshooting (quick queries without console)

Integration with Terraform, Ansible, Python/Boto3

**✅ Summary**

| Category                           | Cost                      |
| ---------------------------------- | ------------------------- |
| IAM users, groups, roles, policies | ✅ Free                    |
| IAM roles for AWS services         | ✅ Free                    |
| IAM Access Analyzer (basic)        | ✅ Free                    |
| STS temporary credentials          | ✅ Free                    |
| CloudTrail logs (for IAM events)   | 💰 Charged for storage    |
| MFA (virtual apps)                 | ✅ Free                    |
| Hardware MFA tokens                | 💰 Charged (if purchased) |

