---
title: AWS Cloud Practitoner Part 01
date: 2025-04-08 04:00:00 +0000
categories: [AWS_Topics]
tags: [cloud, aws]
---

# Compute as a Service (CaaS) in Cloud Computing

## What is Compute as a Service (CaaS)?

**Compute as a Service (CaaS)** is a cloud computing model that delivers virtualized computing resources on demand. CaaS allows organizations to provision processing power — including CPUs, memory, storage, and networking — without investing in physical infrastructure. This model supports scalability, pay-as-you-go pricing, and flexible deployment, making it ideal for modern application development, big data processing, and dynamic workloads.

---

## Key Components of CaaS

### Amazon EC2 (Elastic Compute Cloud)

**Amazon EC2** is a core CaaS offering by AWS. It provides resizable compute capacity in the cloud, allowing users to launch virtual machines — called **instances** — with varying CPU, memory, and storage configurations. EC2 supports multiple operating systems, and users can scale resources up or down based on demand.

### Virtual Machines (VMs)

At the heart of compute services are **Virtual Machines (VMs)** — software-based emulations of physical computers. Each VM runs its own operating system and applications, sharing the underlying physical hardware through a layer called the **hypervisor**.

### Hypervisors and Virtualization

A **hypervisor** (also known as a virtual machine monitor) is the technology that enables virtualization. It allows multiple VMs to run on a single physical host by allocating hardware resources (CPU, memory, etc.) to each VM. Hypervisors are categorized into:

- **Type 1 (Bare Metal):** Runs directly on hardware (e.g., VMware ESXi, Microsoft Hyper-V).
- **Type 2 (Hosted):** Runs on top of an existing OS (e.g., VirtualBox, VMware Workstation).

In cloud environments like AWS, Type 1 hypervisors are commonly used to achieve high performance and isolation between tenants.


### Tenants in Cloud Computing
In a **multi-tenant** cloud architecture, a tenant refers to a single customer or user (which could be an individual, team, or organization) that consumes services from the cloud provider.


### Multi-Tenancy

**Multi-tenancy** is a core principle of public cloud computing where multiple customers (tenants) share the same physical infrastructure securely and independently. Hypervisors ensure strong isolation between tenants' VMs, while cloud providers implement additional security, network segmentation, and identity management to protect customer data and workloads.


### Client & Server

**Client**: A device/person that accesses a service provided by a server.

**Server**: A device/person that provides services for clients.

---

## Infrastructure Geography

### AWS Regions and Availability Zones

AWS infrastructure is organized into **Regions** and **Availability Zones (AZs)**:

- An **AWS Region** is a geographical location with multiple, physically separated data centers.
- Each **Availability Zone** is an isolated data center within a region, designed to be resilient against failures in other zones.

Deploying across multiple AZs increases availability, fault tolerance, and disaster recovery capabilities.

---

## Deployment Models

### 1. Public Cloud

In the **public cloud**, services are delivered over the internet and shared among multiple organizations. Providers like AWS, Azure, and Google Cloud offer scalable compute resources, typically on a pay-per-use basis.

### 2. Private Cloud

A **private cloud** is dedicated infrastructure operated exclusively for a single organization. It can be hosted on-premises or by a third party and offers enhanced control, security, and compliance.

### 3. Hybrid Cloud

**Hybrid cloud** integrates both public and private cloud environments, enabling data and applications to move between them seamlessly. This model is ideal for businesses needing to balance scalability and regulatory requirements.

### 4. On-Premises

**On-premises** deployments involve hosting infrastructure locally within an organization’s own data center. While offering maximum control, it often lacks the scalability and cost-efficiency of cloud environments.

---


## EC2 instance types

### General Purpose
General purpose instances provide a balanced mix of compute, memory, and networking resources. They are ideal for diverse workloads, like web services, code repositories, and when workload performance is uncertain.

### Compute Optimized
Compute optimized instances are ideal for compute-intensive tasks, such as gaming servers, high performance computing (HPC), machine learning, and scientific modeling.

### Memory Optimized
Memory optimized instances are used for memory-intensive tasks like processing large datasets, data analytics, and databases. They provide fast performance for memory-heavy workloads.

### Accelerated Computing
Accelerated computing instances use hardware accelerators, like graphics processing units (GPUs), to efficiently handle tasks, such as floating-point calculations, graphics processing, and machine learning.

### Storage Optimized
Storage optimized instances are designed for workloads that require high performance for locally stored data, such as large databases, data warehousing, and I/O-intensive applications.


## Scalability and Elasticity in AWS

### 🔹 Scalability (in AWS)
**Scalability** refers to the ability of your website infrastructure to **handle increasing or decreasing workloads** by adding or removing resources.

- **Vertical Scaling**: Increasing the size of the instance (e.g., upgrading from `t3.medium` to `m5.large`).
- **Horizontal Scaling**: Adding more instances to distribute load (e.g., using Auto Scaling Groups with multiple EC2 instances behind an Elastic Load Balancer).

✅ **Goal**: Maintain performance and availability as traffic grows.

---

### 🔹 Elasticity (in AWS)
**Elasticity** is the ability to **automatically adjust resources** in response to real-time changes in demand.

- **Scales out** during high traffic (adds instances or resources).
- **Scales in** during low traffic (removes unnecessary resources to save costs).

✅ **Goal**: Optimize cost-efficiency while maintaining performance.

---

### 📌 In the Context of Your Website:
- If traffic spikes (e.g., a viral blog post), AWS can **scale out** automatically using Auto Scaling and Load Balancing.
- When traffic drops, AWS will **scale in** resources automatically — this is **elasticity** in action.

---

### 📊 Summary Table

| Feature       | Scalability                          | Elasticity                                 |
|---------------|--------------------------------------|--------------------------------------------|
| Focus         | Capacity handling                    | Cost-efficiency & real-time responsiveness |
| Manual/Auto   | Can be manual or automatic           | Typically automatic                        |
| Example in AWS| Auto Scaling, RDS Read Replicas      | Auto Scaling Policies, Lambda, ECS Fargate |

---


## Amazon EC2 Auto Scaling

### 🔹 What is Amazon EC2 Auto Scaling?
**Amazon EC2 Auto Scaling** is a service that **automatically adjusts the number of EC2 instances** in your application based on demand, to ensure availability and cost-efficiency.

---

### 🔹 Key Components

- **Launch Template / Launch Configuration**:  
  Defines how new instances should be launched (AMI, instance type, key pair, etc.).

- **Auto Scaling Group (ASG)**:  
  A group of EC2 instances managed together. It ensures the group runs a specified number of instances (called **desired capacity**).

- **Scaling Policies**:  
  Define how and when to scale (based on metrics like CPU usage, network traffic, etc.).
  
- **Health Checks**:  
  Automatically replace unhealthy instances to maintain availability.

---

### 🔹 Types of Scaling

- **Dynamic Scaling**:  
  Automatically scales based on CloudWatch metrics (e.g., CPU > 70%).

- **Predictive Scaling** *(optional)*:  
  Uses machine learning to predict future traffic and scale proactively.

- **Manual Scaling**:  
  You can manually set the desired instance count.

---

### 🔹 Benefits

✅ **High Availability** – Automatically replaces failed or unhealthy instances.  
✅ **Cost Optimization** – Scales in during low demand to reduce costs.  
✅ **Elasticity** – Responds to real-time traffic changes.  
✅ **Integration** – Works seamlessly with Elastic Load Balancer (ELB), CloudWatch, and other AWS services.

---

### 📌 Example Use Case

For a website hosted on EC2:
- During peak hours, EC2 Auto Scaling adds more instances to handle increased traffic.
- At night or low-traffic periods, it automatically reduces the number of instances to save costs.
- If an instance becomes unhealthy, Auto Scaling replaces it automatically.

---

### 🔧 Typical Configuration Example

- **Minimum Instances**: 2  
- **Maximum Instances**: 10  
- **Desired Capacity**: 4  
- **Scaling Policy**: Add 2 instances if average CPU > 70% for 5 minutes

---

### 🧩 Integration Example

```plaintext
Users → Route 53 → Elastic Load Balancer (ELB) → Auto Scaling Group → EC2 Instances
```


## Load Balancing in AWS (Elastic Load Balancing - ELB)

### 🔹 What is Load Balancing?
**Load Balancing** is the process of distributing incoming network traffic across multiple targets (e.g., EC2 instances, containers, IPs) to ensure:

- High availability
- Fault tolerance
- Better performance
- Efficient use of resources

---

### 🔹 What is Elastic Load Balancing (ELB)?
**Elastic Load Balancing (ELB)** is an AWS service that **automatically distributes incoming application traffic** across multiple targets in one or more Availability Zones (AZs).

---

### 🔹 Types of ELB in AWS

| ELB Type                 | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **Application Load Balancer (ALB)** | Works at Layer 7 (HTTP/HTTPS). Supports path-based and host-based routing. Ideal for web apps & microservices. |
| **Network Load Balancer (NLB)**     | Works at Layer 4 (TCP/UDP). Ultra-low latency. Ideal for high-performance or real-time applications. |
| **Gateway Load Balancer (GWLB)**    | Used for deploying, scaling, and managing third-party virtual appliances (e.g., firewalls). |
| **Classic Load Balancer (CLB)**     | Legacy option supporting Layer 4 and Layer 7 (not recommended for new applications). |

---

### 🔹 Key Features

- **Health Checks**: Automatically checks the health of targets and routes traffic only to healthy instances.
- **Cross-Zone Load Balancing**: Distributes traffic evenly across instances in multiple AZs.
- **SSL Termination**: ELB can manage SSL/TLS certificates to offload decryption from backend servers.
- **Auto Scaling Integration**: Works seamlessly with EC2 Auto Scaling to handle dynamic traffic changes.

---

### 📌 Example Use Case

For your website hosted on EC2:
- Users access your domain (via Route 53).
- ELB receives the traffic and **distributes it across healthy EC2 instances** in an Auto Scaling Group.
- If one EC2 instance fails, ELB automatically routes traffic to the remaining healthy ones.

---

### 🧩 Architecture Diagram (Text Format)

```plaintext
Users → Route 53 (DNS) → Elastic Load Balancer (ALB/NLB) → Auto Scaling Group → EC2 Instances
```

## How ELB and EC2 Auto Scaling Work Together in AWS

**Elastic Load Balancing (ELB)** automatically routes incoming application traffic across multiple resources — such as EC2 instances — to help maintain performance and reliability. Acting as a centralized entry point, the load balancer handles all incoming requests and distributes them evenly to the EC2 instances within an Auto Scaling group.

As traffic patterns change, **Amazon EC2 Auto Scaling** adjusts the number of running instances accordingly. The load balancer remains active throughout, intelligently routing requests only to healthy instances, regardless of how many are running.

While ELB and Auto Scaling are independent AWS services, they are often used together. ELB ensures even traffic distribution and health-based routing, while Auto Scaling dynamically adjusts capacity. Together, they enable applications to automatically scale based on demand while maintaining high availability and consistent user experience.


## ELB Routing Methods in AWS

To optimize traffic distribution and improve application performance, **Elastic Load Balancing (ELB)** supports multiple routing algorithms. These strategies help distribute incoming traffic efficiently across backend servers.

---

### 🔁 Round Robin
Distributes incoming requests sequentially across all available servers in a **cyclic order**.  
✅ **Best for**: Equal-capacity servers and stateless applications.

---

### 📉 Least Connections
Sends new traffic to the server with the **fewest active connections** at the time.  
✅ **Best for**: Applications with **long-lived connections** (e.g., streaming or WebSocket apps).

---

### 🌐 IP Hash
Uses the client’s **IP address** to generate a hash, which determines the specific server to route traffic to.  
✅ **Best for**: Scenarios where **session persistence** is needed (e.g., shopping carts).

---

### ⏱️ Least Response Time
Forwards traffic to the server with the **lowest current response time**, helping reduce latency.  
✅ **Best for**: **Latency-sensitive** workloads (e.g., APIs or real-time apps).

---

### 📌 Summary Table

| Method              | Description                                                  | Best Use Case                         |
|---------------------|--------------------------------------------------------------|----------------------------------------|
| **Round Robin**     | Cycles through servers in order                              | Stateless web apps                     |
| **Least Connections**| Chooses server with fewest current connections              | Long-lived connections (chat, stream) |
| **IP Hash**         | Routes based on hashed client IP                             | Sticky sessions                        |
| **Least Response Time** | Chooses fastest-responding server                        | Low-latency, high-performance apps     |

---


## 📨 Messaging & Event Services in AWS

### Amazon EventBridge

**Amazon EventBridge** is a **serverless event bus** service that connects applications using **event data** from AWS services, integrated SaaS apps, or your custom apps.

- **Event-driven architecture**: Triggers actions based on events (e.g., EC2 state change, S3 upload).
- **Event routing**: Uses rules to route events to targets like Lambda, Step Functions, SNS, SQS, etc.
- **Schema discovery**: Automatically detects and stores event schema.

✅ **Use Case**: Decoupled microservices, audit trails, workflows triggered by AWS service events.

```bash
Example Flow:
S3 File Upload → EventBridge → Lambda → Process File
```

### Amazon SQS

**Amazon SQS** is a message queuing service that facilitates reliable communication between software components. It can send, store, and receive messages at any scale, making sure messages are not lost and that other services don't need to be available for processing. In Amazon SQS, an application places messages into a queue, and a user or service retrieves the message, processes it, and then removes it from the queue.


### Amazon SNS

**Amazon SNS** is a publish-subscribe service that publishers use to send messages to subscribers through SNS topics. In Amazon SNS, subscribers can include web servers, email addresses, Lambda functions, and various other endpoints. You will learn about Lambda in more detail later.


## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
