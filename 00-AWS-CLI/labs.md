Lab 1: Install AWS CLI

Install AWS CLI (Linux/Windows/Mac)

Verify installation:

aws --version


👉 Expected Result: Installed version is displayed.

Lab 2: Configure CLI with IAM User

In AWS Console → Create IAM User → Programmatic access → Attach AmazonS3ReadOnlyAccess

Download CSV with Access Key + Secret Key

Run:

aws configure


Verify identity:

aws sts get-caller-identity


👉 Expected Result: Displays IAM user details (UserId, Account, ARN).

Lab 3: Run Basic Commands
aws s3 ls          # List all S3 buckets
aws iam list-users # List IAM users


👉 Expected Result: Shows S3 buckets and IAM users.

Lab 4: Configure Multiple Profiles

Create another IAM user (e.g., prod)

Configure:

aws configure --profile prod


Use specific profile:

aws s3 ls --profile prod


👉 Expected Result: Uses prod user credentials.

Lab 5: Switch Profile with Environment Variable
export AWS_PROFILE=dev   # Linux/Mac
setx AWS_PROFILE dev     # Windows
aws s3 ls                # Runs with dev profile


👉 Expected Result: Uses the selected profile without --profile.
