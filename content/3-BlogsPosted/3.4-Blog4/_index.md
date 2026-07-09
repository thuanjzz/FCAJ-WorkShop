---
title: "Deep Dive into AWS Bedrock & Vector Embedding – How Raw Text Becomes AI \"Coordinates\""
date: 2026-07-09
weight: 4
chapter: false
---

Hello everyone in the AWS Study Group VN community.

Coming from a background of building traditional Backend workflows in Java and Node.js, I recently started diving deeper into Cloud AI and RAG (Retrieval-Augmented Generation) architecture — and actually building a real project that required optimizing this architecture. That's when I stumbled upon a rather fascinating truth: **AI doesn't actually read text the way humans do.**

We often hear about feeding documents to AI and asking questions. But behind the scenes, the system doesn't read strings the way a person reads a book. It requires a critical transformation step called **Embedding**.

Today, I want to dissect the process of using **Amazon Bedrock** to convert raw data into Vectors — the core of every modern AI system.

---

### The RAG Pipeline: A Backend Developer's Familiar Territory

To build a complete RAG system on AWS, your data goes through a processing pipeline that Backend developers will find very familiar:

#### 1. Chunking — Breaking Data Into Pieces

You can't throw a 100-page PDF into Bedrock and tell it to "embed this." That's like trying to swallow an entire chicken whole.

We must use code to split documents into small chunks — roughly a few sentences or one paragraph each. This granular splitting allows the AI to search for information more precisely later, instead of drowning in diluted context.

#### 2. Calling Amazon Bedrock

Once we have the small text segments, the Backend system calls the API and sends each segment to Amazon Bedrock (using specialized models like **Titan Text Embeddings** or **Cohere**).

Bedrock acts as a "magical black box." It takes in a string (e.g., "Return policy within 7 days") and returns a massive array of floating-point numbers (e.g., `[0.012, -0.453, 0.887, ...]`).

#### 3. Storing in a Vector Database

This array of numbers (the Vector) could theoretically be stuffed into a JSON or BLOB column in regular MySQL or PostgreSQL — nothing technically prevents that. The problem is elsewhere: when performing **similarity search** across millions of vectors, traditional databases lack appropriate index mechanisms (like HNSW or IVFFlat), forcing a full sequential scan — extremely slow and resource-hungry.

In practice, people use dedicated databases designed for high-dimensional spaces like **Amazon OpenSearch Serverless**, or add a specialized extension like **pgvector** to PostgreSQL for ANN (Approximate Nearest Neighbor) indexing, making searches dramatically faster than full table scans.

Each record at this point contains: `[Original Text Chunk]` + `[Vector Array]`.

![RAG Pipeline Architecture — Indexing & Query Flow](/images/3-BlogsPosted/3.4-Blog4/blog4.png)

---

### When a User Asks a Question — How Does AI Search?

This is the most fascinating part of RAG architecture.

When a user types: *"I want the ABCXYZ outfit"*, the system executes the following steps:

1. **Embed the query:** Send the question to Bedrock to convert it into a Vector — a new coordinate on the map.
2. **Search the Vector DB:** Take that coordinate into the Vector Database and run **Cosine Similarity** — measuring the angle between two vectors, not a straight-line distance.
3. **Find nearest neighbors:** In plain language, the database uses an angular ruler to identify which documents on that thousands-of-dimensions map sit closest to the query's coordinates.
4. **Retrieve top results:** Pull the top 5 nearest results (the text chunks about short-sleeved shirts, shorts, etc.) and feed that text to the Large Language Model (LLM) to synthesize a natural response.

**All of this happens in just a few hundred milliseconds.**

---

### Why AWS Bedrock Instead of Self-Hosting a Model?

From a system architecture perspective, self-hosting embedding models using open-source libraries on EC2 is a nightmare. You have to manage GPU installation, environment configuration, and especially solve the Auto-scaling problem when request volume spikes.

With **Amazon Bedrock**, the infrastructure is almost entirely managed by AWS (managed/serverless). You simply configure the SDK in Java or Node.js, call the API, and AWS automatically handles infrastructure scaling within your account quota. You pay only for what you use. This lets developers focus entirely on building business logic rather than becoming plumbers for infrastructure.

---

### Battle Scars — Real Lessons from Integrating Bedrock Embedding

**Rate Limit Errors (Throttling):** When you write a script to read thousands of document pages and continuously call the Bedrock API to convert them to Vectors, you'll easily hit API quota limits (per account/region). Always implement a **Retry mechanism** (wait a few seconds before retrying) or use a queue (**Amazon SQS**) to throttle calls.

**Chunking Strategy Is a Matter of Survival:** Cutting text too short causes the AI to lose context (it doesn't understand what's being discussed). Cutting too long causes confusion during search. The practical advice: let text chunks **overlap** slightly with each other.

**Vector DB Costs Money Too:** Storing Vectors consumes significantly more RAM than storing plain text. Calculate costs carefully when choosing your storage service.

**API Call Costs Add Up:** Beyond Vector DB storage costs, calling Bedrock for bulk embedding (especially re-embedding after changing chunking strategy) is billed per token processed. With large datasets, this accumulates fast — think carefully before running batch jobs across your entire document set multiple times.

---

### Conclusion

Amazon Bedrock isn't just a place to host impressive models like Claude or Llama — it also provides extremely powerful Embedding tools to breathe intelligence into raw data.

Mastering the **Chunking → Embedding → Vector Search** pipeline is the key that allows a Backend Developer to confidently build sophisticated AI features without needing to be a mathematics expert or data scientist.

Has anyone here tried building a RAG system and run into problems optimizing Vector DB search results? I'd love to hear real-world experiences from those who've been there before!

***

*   **Original Post:** [AWS Study Group Vietnam — AWS Bedrock & Vector Embedding](https://www.facebook.com/groups/awsstudygroupfcj)
