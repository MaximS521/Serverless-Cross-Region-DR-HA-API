# 🌍 Serverless Cross-Region DR & HA API with AWS

This project demonstrates how to build a **disaster recovery (DR)** and **high availability (HA)** architecture using **AWS serverless services**.  

By combining **Amazon DynamoDB Global Tables**, **AWS Lambda**, **API Gateway**, and **CloudFront**, we create a **fault-tolerant, cross-region API** that can automatically fail over between AWS regions in the event of an outage.

---

## 🚀 Overview

The solution follows **5 major steps**:

1. **Step 1: DynamoDB Global Tables**
   - Created a DynamoDB table with **multi-region replication**.
   - Verified replication between `us-east-1` (primary) and `us-west-2` (secondary).
   - **Diagram:** `diagrams/step1-dynamodb.png`  
   - **Screenshots:** table creation, CLI replication proof.

2. **Step 2: IAM Role for Lambda**
   - Created an **IAM Role** with policies to allow Lambda to interact with DynamoDB.
   - Attached `AWSLambdaBasicExecutionRole` and custom DynamoDB access.
   - **Diagram:** `diagrams/step2-iam-lambda-dynamodb.png`  
   - **Screenshots:** inline policy, role summary.

3. **Step 3: Lambda Functions**
   - Built two Lambda functions:
     - `ReadFunction` → Reads items from DynamoDB.
     - `WriteFunction` → Writes items into DynamoDB.
   - Both configured with the IAM role from Step 2.
   - **Diagram:** `diagrams/step3-lambda-dynamodb.png`  
   - **Screenshots:** function code, test execution.

4. **Step 4: API Gateway**
   - Exposed Lambda functions via REST API endpoints:
     - `/read` → GET → DynamoDB Read Lambda
     - `/write` → POST → DynamoDB Write Lambda
   - Enabled **CORS** and tested via API Gateway console.
   - **Diagram:** `diagrams/step4-api-lambda-dynamodb.png`  
   - **Screenshots:** method integration, CORS enabled, deployment, invoke test.

5. **Step 5: CloudFront with Failover**
   - Configured **CloudFront Distribution** with:
     - Primary origin → API Gateway in `us-east-1`
     - Failover origin → API Gateway in `us-west-2`
   - Failover criteria: 500, 502, 503, 504 errors.
   - **Diagram:** `diagrams/step5-cloudfront-final.png`  
   - **Screenshots:** distribution creation, origin group, failover setup.

---

## 📊 Final Architecture

The final architecture ensures:
- **Global availability** (multi-region setup).
- **Automatic failover** via CloudFront origin groups.
- **Low-latency access** via CloudFront edge locations.
Client Request → CloudFront → Origin Group → (API Gateway + Lambda + DynamoDB Global Table)

---

## 🖼️ Visuals

- **Diagrams:** See the `/diagrams` folder for high-level system diagrams.
- **Screenshots:** Step-by-step proofs available in `/docs/screenshots`.

---

## 🛠️ Tech Stack

- **AWS DynamoDB** – Global Tables for cross-region data replication  
- **AWS Lambda** – Serverless compute functions  
- **Amazon API Gateway** – REST API for Lambda integration  
- **Amazon CloudFront** – CDN + failover routing  
- **IAM** – Role-based access control  

---
