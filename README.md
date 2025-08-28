# Serverless-Cross-Region-DR-HA-API

This project demonstrates how to build a **serverless, cross-region Disaster Recovery (DR) and High Availability (HA) API** using AWS services.  
The architecture ensures that API requests automatically fail over between **us-east-1** and **us-west-2** regions using **CloudFront Origin Groups**.

---

## 🚀 Architecture Overview

- **DynamoDB Global Tables** (active-active replication across `us-east-1` and `us-west-2`).
- **Lambda Functions** for `Read` and `Write` operations.
- **API Gateway** exposing `/read` (GET) and `/write` (POST) endpoints.
- **IAM Roles & Policies** (least privilege for Lambda + DynamoDB).
- **CloudFront Distribution** with failover origin group:
  - Primary Origin → API Gateway in `us-east-1`
  - Failover Origin → API Gateway in `us-west-2` (triggered on 5xx errors).

---

## 📖 Step-by-Step Implementation

### Step 1 — DynamoDB Global Table
- Created **Global Table** spanning `us-east-1` and `us-west-2`.
- Verified replication using CLI (`put-item` + `scan`).

📸 See screenshots in `step1-dynamodb/` and diagram `diagrams/step1-dynamodb.png`.

---

### Step 2 — IAM Role & Policies
- Created **LambdaDynamoDBRole** with:
  - `AWSLambdaBasicExecutionRole`
  - Custom inline policy for DynamoDB access.

📸 See `step2-iam/` and diagram `diagrams/step2-iam-lambda-dynamodb.png`.

---

### Step 3 — Lambda Functions
- **WriteFunction**: Inserts items into DynamoDB.
- **ReadFunction**: Scans items from DynamoDB.

📸 Code + test screenshots in `step3-lambda/`.  
Diagram: `diagrams/step3-lambda-dynamodb.png`.

---

### Step 4 — API Gateway
- Created **HighAvailabilityAPI**.
- Integrated `/read` → `ReadFunction` (GET).
- Integrated `/write` → `WriteFunction` (POST).
- Enabled **CORS** for cross-origin requests.
- Tested using API Gateway test console.

📸 Proof screenshots in `step4-api/`.  
Diagram: `diagrams/step4-api-lambda-dynamodb.png`.

---

### Step 5 — CloudFront Distribution
- Created **CloudFront distribution** with failover:
  - `api-east` → API Gateway in `us-east-1`
  - `api-west` → API Gateway in `us-west-2`
- Configured **Origin Group (api-failover-group)** with failover on **5xx errors**.

📸 Screenshots in `step5-cloudfront/`.  
Diagram: `diagrams/step5-cloudfront.png`.

---

## ✅ Results & Testing

- API requests go to **us-east-1** by default.  
- If `us-east-1` fails (Lambda/API Gateway returns 5xx), CloudFront routes traffic to **us-west-2**.  
- Verified with CloudFront distribution endpoint.

---

## 🔑 Key Takeaways
- Built a **fault-tolerant, highly available serverless API**.
- Learned to integrate **DynamoDB Global Tables, Lambda, API Gateway, IAM, and CloudFront**.
- Demonstrated **multi-region DR/HA** pattern on AWS.

---

## 📌 Next Steps
- Add monitoring (CloudWatch, X-Ray).
- Add API keys or Cognito for authentication.
- Automate setup with Terraform or AWS CDK.

---
