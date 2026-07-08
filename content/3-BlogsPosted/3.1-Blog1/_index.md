---
title: "Create your first search application with Amazon OpenSearch Service"
date: 2026-07-05
weight: 1
chapter: false
---

In the era of data explosion, the ability to search and analyze information in real-time is no longer a luxury but a vital requirement for businesses. From industrial IoT sensors transmitting millions of metrics per second, to e-commerce platforms needing instant product display, or security teams detecting real-time threats—all require a powerful search platform.

**Amazon OpenSearch Service** was born to solve exactly that problem.

### What is OpenSearch Service?

It is a fully managed search and analysis service by AWS. Instead of worrying about infrastructure, you only need to focus on building your application. The **Amazon OpenSearch Serverless** version goes even further by automatically scaling resources according to demand without managing servers.

### Core Architecture to Know

Before diving into code, you need to understand a few foundational concepts:

*   **Document & Index:** The basic data unit is a document (JSON format), organized into indexes, which are similar to tables in a database.
*   **Cluster & Node:** OpenSearch operates on a distributed model. Master nodes manage the cluster, data nodes handle storage and queries, and coordinator nodes route requests to offload data nodes.
*   **Shard & Replica:** Indexes are split into shards (recommended 10–50 GB per shard) and replicated to ensure high availability.
*   **Inverted Index & BM25:** Full-text search technology is powered by the inverted index structure and the BM25 ranking algorithm.

### Sample Application Architecture

A complete search application on AWS is built with a multi-layered security model, where components coordinate tightly from the frontend to the data cluster.

![Sample Application Architecture](/images/3-BlogsPosted/3.1-Blog1/opensearch_architecture.png)

#### Detailed Workflow

When a user interacts with the application, every request is processed in a clear sequence of steps:

1.  **Step 1 - Access UI:** Users open the application via AWS App Runner, where the frontend is hosted and served automatically.
2.  **Step 2 - Authentication:** Amazon Cognito handles identity verification and authorization, ensuring only valid users can proceed.
3.  **Step 3 - Routing via API Gateway:** All requests from the frontend pass through API Gateway—the single entry point for the entire system. API Gateway checks the token from Cognito and forwards the request to Lambda functions inside the VPC.
4.  **Step 4 - Logic Processing in Lambda:** Depending on the request type, AWS Lambda performs one of two tasks: indexing new data into OpenSearch, or executing search queries on the existing cluster.
5.  **Step 5 - OpenSearch in Secure Zone:** The entire OpenSearch cluster resides in the private subnet of the VPC, completely isolated from the internet—a critical security layer for sensitive data.

### Conclusion

With this architecture, you can build a production-ready search application on AWS without managing complex infrastructure. Amazon OpenSearch Service delivers:

*   Fast and scalable search based on actual business needs.
*   Built-in security and compliance without manual configuration.
*   Automated cluster management—AWS handles the heavy lifting, you focus on the product.
*   Flexible pricing model—only pay for what you actually use.

The complete source code and detailed deployment instructions are available at [sample-for-amazon-opensearch-service-tutorials-101](https://github.com/aws-samples/sample-for-amazon-opensearch-service-tutorials-101) on GitHub - you can clone it and try it today.

***

*   **Original Blog Link:** [Amazon OpenSearch Service 101: Create your first search application with OpenSearch](https://aws.amazon.com/blogs/big-data/amazon-opensearch-service-101-create-your-first-search-application-with-opensearch/)
