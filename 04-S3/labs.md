**🧪 AWS S3 – Hands-On Labs (with Full Explanation)**
**Lab 1 – Create an S3 Bucket**
🎯 Goal: Create your own bucket to store data objects.

🧭 Steps
        Open AWS Console → S3 → Create bucket
        Configure:
        Bucket name: my-first-s3-bucket-<unique-name>
        Region: ap-south-1 (or your nearest region)
        Leave defaults and click Create bucket
        💡 Explanation: Every bucket name must be globally unique.
        The region controls latency and pricing.

✅ Expected Result: Your new bucket appears in S3 list.

**Lab 2 – Upload Objects & Understand Object Structure**

🎯 Goal: Upload a file and understand key-value object storage.

        🧭 Steps
        Open bucket → Upload → select any file (e.g., test.txt) → Upload
        Click the object → see Properties and Metadata
        💡 Explanation:
        Each object = Key (name) + Value (data) + Metadata.

✅ Expected Result: File stored successfully and visible in S3.

**Lab 3 – Enable Versioning**

🎯 Goal: Keep multiple versions of the same file.

        🧭 Steps
        Bucket → Properties → Versioning → Enable
        Upload another file with same name (test.txt) → overwrites object
        Click Versions tab → see multiple entries.
        💡 Explanation: Versioning protects from accidental overwrites and deletes.

✅ Expected Result: Each file has a unique Version ID.

**Lab 4 – Default Encryption for Data at Rest**

🎯 Goal: Encrypt data automatically when stored.

        🧭 Steps
        Bucket → Properties → Default encryption
        Choose either SSE-S3 (AWS-managed keys) or SSE-KMS (KMS keys).
        Save → Upload new file → Check Encryption status.
        💡 Explanation: S3 encrypts data before writing and decrypts on read.

✅ Expected Result: All new objects show “Encrypted = Yes”.

**Lab 5 – Set Bucket Policy for Public Read Access**

🎯 Goal: Allow public read for objects (e.g., website hosting).
        
        🧭 Steps
        Bucket → Permissions → Bucket policy → Edit
        Paste this JSON (replace bucket name):
        {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Sid": "PublicReadGetObject",
              "Effect": "Allow",
              "Principal": "*",
              "Action": ["s3:GetObject"],
              "Resource": ["arn:aws:s3:::my-first-s3-bucket-12345/*"]
            }
          ]
        }
        
        Save → upload an image → copy Object URL → open in browser.
        
        💡 Explanation: This policy allows read-only access to anyone on the Internet.

✅ Expected Result: Image loads publicly via URL.

**Lab 6 – Enable Static Website Hosting**

🎯 Goal: Host a simple HTML site using S3.

        🧭 Steps
        Create index.html file → upload to bucket.
        Bucket → Properties → Static website hosting → Enable
        Choose “Host a static website” and enter:
        Index document: index.html
        Copy the Website Endpoint URL.
        💡 Explanation: S3 serves static content directly over HTTP/HTTPS.

✅ Expected Result: Your HTML page loads from the S3 endpoint.

**Lab 7 – Access S3 via CLI**

🎯 Goal: Interact with S3 using AWS CLI commands.
        
        🧭 Commands
        
        aws s3 ls
        aws s3 mb s3://my-cli-bucket-12345
        aws s3 cp sample.txt s3://my-cli-bucket-12345/
        aws s3 ls s3://my-cli-bucket-12345/
        aws s3 rb s3://my-cli-bucket-12345 --force
        
        💡 Explanation: S3 CLI commands follow aws s3 <operation> pattern.

✅ Expected Result: Buckets and objects managed via command line.

**Lab 8 – Set Lifecycle Policy**

🎯 Goal: Automatically move objects to Glacier or delete after days.

        🧭 Steps
        Bucket → Management → Lifecycle rules → Create rule
        Name: Move-to-Glacier
        Choose “All objects” → Transition after 30 days to Glacier Deep Archive.
        Save rule.
        
        💡 Explanation: Lifecycle rules automate cost optimization.

✅ Expected Result: Objects older than 30 days automatically move to Glacier.

**Lab 9 – Cross-Region Replication (CRR)**

🎯 Goal: Replicate objects between buckets in different regions.
        
        🧭 Steps
        Create 2 buckets in different regions.
        Enable versioning in both.
        Source bucket → Management → Replication rules → Add rule
        Select Destination bucket → Enable rule.
        💡 Explanation: CRR copies objects asynchronously for disaster recovery and low-latency access.

✅ Expected Result: New objects in source are auto-replicated to destination.

**Lab 10 – Clean Up**

🎯 Goal: Avoid unnecessary charges.

        🧭 Stepsll uploaded files and buckets.
        Remove CRR rules and disable versioning if not needed.
