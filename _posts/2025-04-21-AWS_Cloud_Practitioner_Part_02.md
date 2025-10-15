---
title: AWS Cloud Practitoner Part 02
date: 2025-04-21 04:00:00 +0000
categories: [AWS_Topics]
tags: [cloud, aws]
---


# 🧠 AWS: Compute Options for Website Deployment (Service Models, Lambda, Containers, and More)

---

## 📌 Overview

When deploying a website on AWS, choosing the right **compute model** is crucial. Depending on your **team size, scaling needs, and operational preferences**, AWS offers:

1. **Unmanaged services** (you manage everything)  
2. **Managed services** (shared responsibility)  
3. **Fully managed services** (AWS handles it all)  
4. **Containers & Orchestration** (balance of control and abstraction)  
5. **Other compute services** (for specific use cases)

This guide explains each with **technical details** and **interview insights**.

---

## 🧱 1. AWS Service Management Levels

| Type               | Control Level | Ops Responsibility      | Examples                                 | Best Use Case                           |
|--------------------|---------------|--------------------------|------------------------------------------|------------------------------------------|
| **Unmanaged**       | Full           | You handle everything     | EC2, EBS                                  | Custom setups, full control              |
| **Managed**         | Partial        | AWS manages infra         | RDS, Elastic Beanstalk, ECS              | Common web stacks, moderate flexibility  |
| **Fully Managed**   | Minimal        | AWS handles it all        | Lambda, S3, DynamoDB, Amplify             | Fast development, minimal ops            |

---

## 🔧 2. Unmanaged Services

### ✅ EC2 for Web Apps

- Launch VMs and install everything: web servers, runtimes, DBs.
- Full responsibility for OS updates, security, scaling, backups.

### 🧠 Interview Insight:
> "EC2 gives me full control, ideal for legacy or custom apps. But it comes with high operational overhead."

---

## ⚙️ 3. Managed Services

### ✅ Elastic Beanstalk

- Platform-as-a-Service (PaaS) for web apps.
- You deploy code; AWS handles provisioning, scaling, load balancing.
- Works with Java, Python, Node.js, PHP, Ruby, Go, .NET.

#### 🧠 Interview Insight:
> "Elastic Beanstalk helps deploy web apps quickly with minimal configuration while still giving access to EC2 under the hood."

---

### ✅ RDS

- Managed relational databases (MySQL, PostgreSQL, SQL Server).
- Backups, patches, failover, Multi-AZ support.

---

### ✅ Amazon ECS (Elastic Container Service)

- Managed container orchestration.
- Can run on EC2 or Fargate (serverless containers).
- Works well with ALB, IAM, CloudWatch.

---

### ✅ AWS Fargate

- Serverless compute engine for ECS and EKS.
- No server management; just define container specs.

---

## ☁️ 4. Fully Managed Services

### ✅ AWS Lambda

- Run code without provisioning servers.
- Event-driven, auto-scaling, stateless.
- Ideal for APIs, file processing, microservices.

---

### ✅ Amazon S3 + CloudFront

- S3 hosts static frontend (HTML/JS/CSS).
- CloudFront caches globally with HTTPS support.

---

### ✅ AWS Amplify

- Full-stack app deployment (frontend + backend).
- Supports hosting, GraphQL/REST APIs, auth (Cognito), and CI/CD.

---

## 🧪 Lambda Deep Dive

| Feature         | Details                               |
|-----------------|----------------------------------------|
| Stateless       | No persistence between runs            |
| Max Timeout     | 15 minutes                             |
| Memory          | 128 MB – 10 GB                         |
| Languages       | Node.js, Python, Java, Go, etc.        |
| Cold Start      | Latency during first invocation        |

---

### 🔗 Lambda Triggers

- API Gateway
- S3
- DynamoDB Streams
- EventBridge
- SQS/SNS

---

### 🧠 Lambda Interview Insights

**Q: When do you use Lambda?**  
> "For event-driven use cases like APIs, file processing, or data pipelines."

**Q: What are cold starts?**  
> "Cold starts happen when a new Lambda instance initializes. Use provisioned concurrency to reduce latency."

---

### 🏗️ Website Architecture with Lambda

```text
[Frontend (React/S3)]
          ↓
 [Amazon API Gateway]
          ↓
     [Lambda Functions]
          ↓
 [DynamoDB / RDS / S3]
```

---

## 🐳 5. Containers & Orchestration

Containers give more flexibility than Lambda and more efficiency than EC2.

### ✅ ECS (Elastic Container Service)

- AWS-native container orchestration.
- Integrates with EC2 or Fargate.
- Define task definitions, services, and clusters.

---

### ✅ AWS EKS (Elastic Kubernetes Service)

- Managed Kubernetes control plane.
- Use `kubectl`, Helm, ArgoCD, etc.
- More complex but ideal for cloud-agnostic strategies.

---

### 🧠 Container Interview Insights

**Q: ECS vs EKS?**  
> "ECS is simpler and deeply integrated with AWS. EKS is powerful but better for teams with Kubernetes experience."

**Q: ECS (Fargate) vs Lambda?**  
> "Use ECS Fargate when you need container flexibility or longer runtimes, and Lambda when you want minimal infrastructure."

---

### 🏗️ Website Architecture with Containers

```text
[Frontend (React/S3/ALB)]
          ↓
[ECS/EKS (Fargate) App Services]
          ↓
[RDS / DynamoDB / ElastiCache / S3]
```

---

## ⚙️ 6. Other AWS Compute Services

---

### ✅ Elastic Beanstalk (Recap)

- Deploy web apps without managing infrastructure.
- Control platform versions, scaling settings, health checks.
- Gives access to underlying EC2 if needed.

---

### ✅ AWS Batch

- Fully managed batch processing at scale.
- Automatically provisions compute resources.
- Ideal for data pipelines, ML training jobs, and simulations.

#### 🧠 Interview Insight:
> "I use AWS Batch when I need to run large-scale, parallel, non-interactive compute jobs without managing queues or servers."

---

### ✅ Amazon Lightsail

- Simplified cloud VPS with predictable pricing.
- Pre-configured stacks: WordPress, LAMP, Node.js, etc.
- Includes networking, DNS, snapshots.

#### 🧠 Interview Insight:
> "Lightsail is perfect for small apps or quick MVPs. It’s simpler than EC2 and includes everything from DNS to firewall."

---

### ✅ AWS Outposts

- AWS infrastructure deployed **on-premises**.
- Run EC2, RDS, S3 locally with same AWS APIs.
- Ideal for low-latency or data residency use cases.

#### 🧠 Interview Insight:
> "Outposts is for hybrid workloads where local processing is needed but AWS consistency must be preserved."

---

## 🔚 Final Decision Table: Which to Choose?

| Service            | When to Use                                                            |
|--------------------|------------------------------------------------------------------------|
| **EC2**            | Full control, custom OS, legacy or licensed apps                       |
| **Elastic Beanstalk** | Simplified app deployment without deep ops knowledge                  |
| **Lambda**         | Event-driven, short-lived tasks with no server management              |
| **ECS/Fargate**    | Dockerized workloads with control over runtime                         |
| **EKS**            | Kubernetes-native teams or multi-cloud strategy                        |
| **AWS Batch**      | Parallel job processing, ML model training, simulations                |
| **Lightsail**      | Quick, low-cost VPS with simple app hosting                            |
| **Outposts**       | Hybrid infrastructure or local data compliance requirements            |

---

## 🛠️ Bonus: Deployment Stack Examples

### ✅ Serverless Web App (Frontend + Backend)

```text
[React Frontend - S3 + CloudFront]
          ↓
 [Amazon API Gateway]
          ↓
       [Lambda]
          ↓
[DynamoDB / Aurora Serverless / S3]
```

---

### ✅ Containerized Web App

```text
[React or Next.js Frontend - S3 or ALB]
          ↓
 [Backend on ECS or EKS (Fargate)]
          ↓
[RDS / ElastiCache / DynamoDB / S3]
```

---

### ✅ Batch or Scheduled App

```text
[EventBridge Schedule]
          ↓
        [AWS Batch]
          ↓
[Process large jobs / transform / export data]
```

---

> 🧠 **Pro Interview Tip**: Show you understand trade-offs:
- Lambda is fastest to deploy but stateless and time-limited.
- ECS gives flexibility with containerization.
- EKS offers control but requires expertise.
- Lightsail and Beanstalk are great for bootstrapping.


## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
