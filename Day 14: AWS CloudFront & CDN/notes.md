# ☁️ Day 14: AWS CloudFront & Content Delivery Network (CDN) – Theory

## 🧠 1. What is AWS CloudFront?

**Amazon CloudFront** is a **Content Delivery Network (CDN)** service provided by AWS.  
It delivers your web content (HTML, CSS, JS, images, videos, etc.) to users **faster** and **more securely** by caching copies of your content at **AWS Edge Locations** around the world.

When a user requests your content:
- The request goes to the nearest edge location.
- If the content is cached, CloudFront serves it immediately.
- If not, it fetches it from your origin (like an S3 bucket or EC2 server), caches it, and serves it to the user.

This improves **speed**, **availability**, and **reduces load** on your origin servers.

---

## 🌍 2. How CloudFront Works (Step-by-Step)

1. You have your **origin** (e.g., S3 bucket, EC2, or ALB).
2. You create a **CloudFront Distribution** linked to that origin.
3. CloudFront replicates your content to **Edge Locations** (over 400+ globally).
4. A user requests your content via a **CloudFront URL** (e.g., `d123.cloudfront.net/image.png`).
5. The nearest **Edge Location** serves the cached content or fetches from origin if not cached.

✅ **Result:** Low latency, fast delivery, and optimized user experience.

---

## 🧩 3. Key CloudFront Components

| Component | Description |
|------------|--------------|
| **Origin** | The source of your content (e.g., S3 bucket, EC2 instance, or Load Balancer). |
| **Edge Location** | Global data centers that cache content closer to users. |
| **Distribution** | The CloudFront setup that defines origin, caching behavior, and delivery settings. |
| **Cache Behavior** | Rules for how CloudFront caches and delivers specific URLs or paths. |
| **TTL (Time To Live)** | How long CloudFront caches the content before checking with the origin. |
| **Invalidation** | A manual request to remove or refresh cached objects from Edge Locations. |
| **Signed URLs/Cookies** | Used to restrict access to premium or private content. |

---

## 🏗️ 4. CloudFront Architecture Overview

User Request → Edge Location → CloudFront Cache
↓ (Cache Miss)
Origin Server (S3, EC2, ALB)


- If the object exists in cache → served instantly.
- If not → CloudFront retrieves from origin → caches it → serves to user.
- Next requests from nearby users are served **locally** (no origin hit).

---

## ⚙️ 5. Common CloudFront Use Cases

| Use Case | Description |
|-----------|--------------|
| **Website acceleration** | Serve static/dynamic content globally with low latency. |
| **Video streaming** | Stream live or on-demand videos to users worldwide. |
| **S3 website hosting** | Use CloudFront in front of an S3 bucket for speed & HTTPS support. |
| **API acceleration** | Cache API responses to reduce backend load. |
| **Security** | Integrate with AWS WAF, Shield, and SSL/TLS for DDoS and HTTPS protection. |

---

## 🔐 6. Security Features

- **HTTPS (SSL/TLS)**: CloudFront enforces encryption between users and the edge.
- **AWS WAF Integration**: Add a firewall layer to block malicious requests.
- **Signed URLs / Signed Cookies**: Restrict access to specific content (useful for premium videos or downloads).
- **Geo-Restriction**: Allow or deny content access based on the viewer’s country.
- **Origin Access Control (OAC)**: Restrict direct S3 access — users can only fetch content through CloudFront.

---

## 💰 7. Pricing Model

CloudFront pricing depends on:
1. **Data transfer out** to the Internet (varies by region)
2. **Requests** (GET, PUT, etc.)
3. **Invalidation requests** (first 1,000 paths free per month)
4. **Additional features** like Lambda@Edge execution

💡 *Tip:* It’s free for the first **1 TB** per month under the **AWS Free Tier**.

---

## 🧮 8. Performance Optimization

| Optimization | Description |
|---------------|--------------|
| **Compress objects automatically** | Reduce file size for text-based assets (HTML, CSS, JS). |
| **Use Origin Shield** | Add an extra cache layer for large-scale applications. |
| **Use custom TTL values** | Control cache refresh time per path. |
| **Set proper cache behaviors** | Cache static content heavily, dynamic content lightly. |

---

## 💡 9. Integration Examples

- **S3 + CloudFront** → Global static website hosting  
- **EC2/ALB + CloudFront** → Secure and fast dynamic site  
- **API Gateway + CloudFront** → Cached API responses  
- **Lambda@Edge + CloudFront** → Custom logic at edge (e.g., header manipulation, redirects)

---

## 🧠 10. Common Interview Questions

1. What is CloudFront and why is it used?
2. How does CloudFront improve website performance?
3. What is the difference between **Edge Location** and **Region**?
4. What are **signed URLs** and **signed cookies**?
5. What is **Origin Access Control (OAC)** vs **Origin Access Identity (OAI)**?
6. What happens if CloudFront cache expires?
7. How do you clear specific cached content in CloudFront?
8. Can CloudFront serve both static and dynamic content?

---

## 🏁 Summary

- **CloudFront = AWS CDN service**
- Caches and delivers content globally with low latency
- Works with **S3, EC2, ALB, API Gateway**
- Integrates with **AWS WAF**, **Shield**, and **Lambda@Edge**
- Supports **custom caching rules**, **HTTPS**, **Geo-restrictions**, and **signed URLs**

✅ Improves performance, reduces cost, and adds security to your global content delivery.

Would you like me to now create the CloudFront Labs (Hands-On) next (in GitHub format) — where we’ll:

Set up a CloudFront distribution for an S3-hosted website

Configure HTTPS, cache behaviors, and OAC

Test Geo-restriction and cache invalidation?
