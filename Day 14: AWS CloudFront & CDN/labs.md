# 🚀 Day 14: CloudFront & CDN – Hands-On Labs

## 🎯 Objective

In this lab, you will:
- Create a static website in **Amazon S3**  
- Distribute it globally using **Amazon CloudFront**  
- Secure it with **HTTPS (SSL)**  
- Test caching, invalidation, and Geo-Restriction  

---

## 🧩 Lab 1: Create an S3 Static Website (Origin)

### 🪜 Steps

1. **Create S3 Bucket**
   - Go to **S3 Console**
   - Click **Create bucket**
   - Name it: `my-cloudfront-demo`
   - Region: `us-east-1`
   - Uncheck “Block all public access”
   - Acknowledge the warning

2. **Upload Website Files**
   - Create two files locally:
     - `index.html`
     - `error.html`
   - Upload both to the bucket

3. **Enable Static Website Hosting**
   - Go to **Properties** tab → **Static website hosting**
   - Enable it
   - Index document: `index.html`
   - Error document: `error.html`
   - Copy the **Endpoint URL** (e.g., `http://my-cloudfront-demo.s3-website-us-east-1.amazonaws.com`)

4. **Set Public Read Policy**
   - Go to **Permissions → Bucket Policy**
   - Add this policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-cloudfront-demo/*"
    }
  ]
}

✅ Test the S3 website link in your browser.

🌍 Lab 2: Create a CloudFront Distribution
🪜 Steps

Open CloudFront Console

Click Create Distribution

Configure Origin Settings

Origin domain: Select your S3 bucket

Origin access: Choose Origin Access Control (OAC)

Create control, and attach it to the origin

This ensures S3 content is accessible only through CloudFront

Default Cache Behavior

Viewer Protocol Policy: Redirect HTTP to HTTPS

Allowed HTTP Methods: GET, HEAD

Cache Policy: CachingOptimized

Distribution Settings

Alternate domain name (optional): e.g., cdn.mywebsite.com

SSL certificate: Choose Default CloudFront certificate (*.cloudfront.net)

Enable Compress objects automatically

Click Create Distribution

⏳ Wait until the Status changes to Deployed

Test Distribution

Copy the CloudFront Domain Name (e.g., d123abcd.cloudfront.net)

Open it in your browser → You’ll see your index.html page

🔐 Lab 3: Enable HTTPS & Restrict S3 Access

Confirm OAC is Active

In CloudFront → Origins → Edit → Verify that “Origin Access Control” is enabled

Update S3 Bucket Policy

Remove any public access policies

Add this minimal policy:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-cloudfront-demo/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::<YOUR_ACCOUNT_ID>:distribution/<DISTRIBUTION_ID>"
        }
      }
    }
  ]
}

✅ Now, your S3 bucket content is only accessible through CloudFront — not directly from the S3 URL.

🌐 Lab 4: Invalidate Cache (Force Refresh)
🪜 Steps

Make a small change in your index.html (e.g., update a text line)

Upload the new version to your S3 bucket

Go to CloudFront Console → Invalidations → Create Invalidation

Path: /*

Click Invalidate

⏳ Wait for status = Completed

✅ Now refresh your CloudFront URL — new content will appear immediately.

🌎 Lab 5: Geo-Restriction (Optional)
🪜 Steps

Go to your CloudFront Distribution

Click Edit → Settings

Under Geo-restrictions

Choose Whitelist or Blacklist

Example: Blacklist → IN (India)

Save changes

✅ Test with a VPN — users from restricted countries will be denied access.

🧠 Lab Summary

| Task                             | Description                | Status |
| -------------------------------- | -------------------------- | ------ |
| ✅ Create S3 static site          | Hosted sample website      | ✔️     |
| ✅ Create CloudFront Distribution | Connected S3 as origin     | ✔️     |
| ✅ Configure HTTPS                | Secure delivery            | ✔️     |
| ✅ Test caching and invalidation  | Updated content instantly  | ✔️     |
| ✅ Geo Restriction                | Controlled regional access | ✔️     |


🧩 Additional Exercises

Use Custom Domain with Route 53 + ACM Certificate

Add Lambda@Edge to modify headers or redirects

Enable Logging for your distribution

Test performance with and without CloudFront using pingdom.com

🏁 Conclusion

By completing this lab, you learned to:

Deliver content globally using Amazon CloudFront

Integrate with S3 as an origin

Secure with HTTPS and restrict access

Manage cache invalidation and Geo-Restrictions

You’re now ready to deploy high-performance, global-scale applications with AWS CloudFront.

