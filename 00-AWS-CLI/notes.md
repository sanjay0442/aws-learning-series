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

Managing multiple AWS accounts

Troubleshooting (quick queries without console)

Integration with Terraform, Ansible, Python/Boto3
