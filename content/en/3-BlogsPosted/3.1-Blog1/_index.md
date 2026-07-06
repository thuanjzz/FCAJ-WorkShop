---
title: "Blog 1"
date: 2026-07-05
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# VPC Endpoints: Secure Private Access to Amazon S3

### 1. Introduction

In modern cloud architectures, security and cost optimization are paramount. When compute resources inside a virtual private cloud (VPC) need to communicate with storage services like Amazon S3, routing traffic through the public internet is a bad practice. It introduces security vulnerabilities and incurs unnecessary NAT Gateway data transfer charges. 

AWS PrivateLink solves this by providing private connectivity using VPC endpoints. This blog post explores how to configure and verify Gateway and Interface VPC Endpoints for secure, private S3 access.

---

### 2. Gateway Endpoints vs. Interface Endpoints

| Feature | Gateway Endpoint | Interface Endpoint |
|---|---|---|
| **Underlying Tech** | Routing table prefix list | Elastic Network Interface (ENI) with Private IP |
| **Access Method** | Direct route table entry | DNS resolution |
| **Scope** | VPC internal only | VPC & On-premises (via VPN/Direct Connect) |
| **Cost** | Free | Pay-per-hour + Data processing |

---

### 3. Setting Up a Gateway VPC Endpoint for S3

For EC2 instances running inside a private subnet within the cloud VPC, a **Gateway Endpoint** is the most cost-effective and performant choice.

#### Step 1: Create the Endpoint
1. Open the **Amazon VPC console**.
2. In the navigation pane, choose **Endpoints** -> **Create endpoint**.
3. Service category: Choose **AWS services**.
4. Service name: Search `s3` and select the **Gateway** type (e.g., `com.amazonaws.us-east-1.s3`).
5. VPC: Select your Cloud VPC.

#### Step 2: Configure Route Tables
1. Under **Route tables**, select the route tables associated with your private subnets.
2. The wizard automatically adds a route pointing to the S3 prefix list (e.g., `pl-63a5400a`) with the target set to the endpoint ID (`vpce-xxxxxx`).

#### Step 3: Verify the Connection
SSH into your private EC2 instance and run:
```bash
aws s3 ls --region us-east-1
```
If configured correctly, the command returns the bucket list immediately without internet gateway access.

---

### 4. Setting Up an Interface VPC Endpoint for Hybrid/On-Premises Access

If you need to access Amazon S3 from an on-premises network via a VPN tunnel or AWS Direct Connect, Gateway Endpoints cannot be targeted. Instead, you must use an **Interface Endpoint**.

#### Step 1: Create the Endpoint
1. In the VPC Console, click **Create endpoint**.
2. Select **AWS services** -> Search `s3` -> Select the **Interface** type.
3. Select your VPC and the specific subnets where the ENIs should be provisioned.
4. Enable **Private DNS** to let application queries automatically resolve to the private endpoint IPs.

#### Step 2: Test from On-Premises
Using the Interface Endpoint's DNS name, query S3 from your on-premises simulator:
```bash
aws s3 ls --endpoint-url https://bucket.vpce-xxxxxx.s3.us-east-1.vpce.amazonaws.com
```

---

### 5. Conclusion

Implementing VPC Endpoints is a fundamental security practice on AWS. By routing S3 traffic internally, you protect your data from public internet exposure and significantly reduce NAT Gateway egress costs.