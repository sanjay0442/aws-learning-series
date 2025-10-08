🧠 Amazon S3 – Theory Notes
**1️⃣ What is Amazon S3?**

  Amazon Simple Storage Service (S3) is an object storage service used to store and retrieve any amount of data — images, logs, backups, videos, etc.
  It’s highly scalable, durable (99.999999999% or 11 9’s), and available worldwide.

**Analogy:**
Think of S3 as a giant online folder system, but more powerful and globally distributed.

**2️⃣ Key S3 Concepts**
    Bucket:	A container for objects (like a top-level folder). Each bucket name must be globally unique.
    Object:	The actual data (file) stored inside a bucket. Each object has data + metadata + key (unique name).
    Key:	The full path to an object within a bucket (like /images/pic1.png).
    Region:	Buckets are created in specific AWS regions (e.g., ap-south-1).
    Storage: Class	Defines how frequently you access data and its cost.
    Versioning:	Keeps multiple versions of the same object.
    Encryption:	Protects data at rest (SSE-S3, SSE-KMS, SSE-C).
    Lifecycle: Rules	Automates moving or deleting objects based on age.
    Static Website Hosting:	You can host HTML websites directly from S3.
    
**3️⃣ S3 Storage Classes**
<img width="1167" height="702" alt="image" src="https://github.com/user-attachments/assets/6445d241-83a6-4a3e-82e6-103c253cf1fe" />


**4️⃣ S3 Security**
  S3 offers multiple layers of security:
  Bucket Policies: JSON-based policies to control access.
  IAM Policies: User/role-level permissions.
  ACLs (Access Control Lists): Legacy access control (not recommended for new setups).
  Public Access Block: Global setting to prevent public exposure.
  Encryption:
        SSE-S3: Managed by AWS.
        SSE-KMS: Uses AWS KMS key (more control).
        Client-side encryption: Encrypt before uploading.

**5️⃣ S3 Versioning**
    Keeps multiple copies of the same object.
    Helps recover from accidental deletion or overwrite.
    Once enabled, cannot be disabled — only suspended.

**6️⃣ S3 Lifecycle Rules**
    Automate transitions and deletions:
    Move Standard → IA → Glacier → Delete.
    Useful for log rotation and archival policies.

**7️⃣ S3 Static Website Hosting**
    S3 can serve as a web host for static content:
    HTML, CSS, JS, images
    Enable “Static Website Hosting” in bucket properties
    Requires public-read policy
    Example URL:
    http://mybucket.s3-website-ap-south-1.amazonaws.com

**8️⃣ S3 Data Transfer**

Upload via:
    AWS Console
    AWS CLI (aws s3 cp, aws s3 sync)
    SDKs (Python boto3, etc.)
    Transfer Acceleration: Uses AWS CloudFront’s edge network for faster global upload.

**9️⃣ S3 Pricing**

You pay for:
    Storage (GB/month)
    Requests (GET, PUT)
    Data Transfer (outbound)
    Management features (e.g., inventory, analytics)

💡 Always enable lifecycle rules and object expiry to control cost.

**🔟 Common Interview Questions**

Difference between S3 and EBS?
    What are S3 storage classes?
    How to restrict bucket access?
    How to access private S3 from private subnet?
    → via VPC Endpoint
    How to host a website on S3?
    How to protect against accidental deletes?
    → Versioning + MFA Delete
    Can we mount S3 as a filesystem?
    → Yes, using tools like s3fs or AWS DataSync.

✅ Summary

    S3 = Object Storage (not block or file storage)
    Highly available, scalable, and secure
    Ideal for backups, websites, logs, big data, and media storage
    Integrates with most AWS services (CloudFront, Lambda, Glacier, etc.)
