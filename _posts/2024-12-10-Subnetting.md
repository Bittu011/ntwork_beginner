---
title: Subnetting
date: 2024-12-10 05:00:00 +0000
categories: [Networking_Topics, CCNA]
tags: [subnetting, networking, ccna, ccnp]
---

# 🏛️ The Rise and Fall of Classful Addressing — A Legacy of the Internet


## 🧠 The Birth of Classful Addressing: Internet’s Early Blueprint

Before the digital world exploded with smartphones, cloud platforms, and smart fridges, the internet needed a way to manage its growing user base. Enter: **Classful Addressing**, a system born in the 1980s that would lay the foundation for early IP allocation.

It was simple. It was structured. But ultimately... it was flawed.

---

## 📦 Understanding the Classes

In Classful Addressing, IPv4 addresses were divided into 5 classes based on their first octet:

| Class | First Octet | Subnet Mask       | Address Range                   | Max Hosts    | Use Case             |
|-------|--------------|-------------------|----------------------------------|--------------|-----------------------|
| A     | 1–126        | 255.0.0.0 (/8)     | 1.0.0.0 – 126.255.255.255        | ~16 million  | Large organizations   |
| B     | 128–191      | 255.255.0.0 (/16)  | 128.0.0.0 – 191.255.255.255      | ~65,000      | Medium-sized networks |
| C     | 192–223      | 255.255.255.0 (/24)| 192.0.0.0 – 223.255.255.255      | 254          | Small networks        |
| D     | 224–239      | N/A               | 224.0.0.0 – 239.255.255.255      | N/A          | Multicast             |
| E     | 240–255      | N/A               | 240.0.0.0 – 255.255.255.255      | N/A          | Research/Testing      |

> 🔁 **Note**: The `127.x.x.x` range is reserved for loopback addresses.

---

## ❌ Why Classful Addressing Failed

As the internet grew, Classful Addressing became inefficient:

| Problem              | Description |
|----------------------|-------------|
| **IP Address Waste** | A company needing 300 IPs would get a Class B (~65,000 IPs). Huge waste. |
| **Fixed Block Sizes**| No flexibility — only /8, /16, /24 were available. |
| **Routing Table Bloat** | Each network needed its own route. Routing tables became massive. |
| **Inefficient Allocation** | Organizations held massive unused IP ranges. |

---

## 💡 Enter CIDR: The Classless Savior

**CIDR** (Classless Inter-Domain Routing) was introduced in **1993 (RFC 1519)** to fix Classful's problems:

✅ Key Features:
- Eliminated rigid classes (A, B, C)
- Uses **prefix notation** (`/13`, `/21`, `/30`)
- Supports **subnetting and supernetting**
- Enables **route summarization**

---

## 🆚 Classful vs Classless (CIDR)

| Feature           | Classful           | CIDR (Classless)         |
|------------------|--------------------|---------------------------|
| Address Division | Fixed (A, B, C)     | Flexible                 |
| Subnet Mask      | Default per class   | Any length (e.g., /27)  |
| IP Allocation    | Wasteful            | Efficient               |
| Routing Tables   | Large and slow      | Summarized/Optimized    |
| Used Today?      | ❌ Deprecated        | ✅ Industry Standard     |

---

## 🧠 TL;DR

- **Classful Addressing** was simple but highly inefficient.
- **CIDR** brought the flexibility, scalability, and efficiency that the modern internet needs.

---

# 🔧 CIDR & Subnetting: A Complete Technical Deep-Dive

> **Category**: Technical / CCNA / Network+ Prep

---

## 1️⃣ What is CIDR?

**CIDR (Classless Inter-Domain Routing)** is the modern method for IP address allocation and routing.

- Uses prefix notation like `/24`, `/27`
- Efficient allocation (no more wasted IPs)
- Supports supernetting and subnetting

### 💡 Example:
`192.168.10.0/26` – `/26` means 26 bits are used for the network portion.

---

## 2️⃣ What is Subnetting?

**Subnetting** divides a larger network into smaller, manageable subnets.

### 🧩 Why Subnet?
- Logical segmentation
- Better performance and security
- Reduced broadcast traffic
- Optimized IP usage

---

## 3️⃣ Subnetting Methods

### 📏 3.1 Fixed-Length Subnet Masking (FLSM)
- All subnets are **equal** in size
- Same subnet mask used everywhere
- Simple, ideal for labs

### 📐 3.2 Variable-Length Subnet Masking (VLSM)
- Subnets have **different sizes**
- Efficient and scalable
- Used in real-world networks

---

## 4️⃣ Subnetting by Borrowing Bits

| Borrowed Bits | Subnets | Hosts/Subnet |
|---------------|---------|--------------|
| 1             | 2       | 126          |
| 2             | 4       | 62           |
| 3             | 8       | 30           |
| 4             | 16      | 14           |
| 5             | 32      | 6            |

> ⚠️ Subtract 2 hosts per subnet (1 for network, 1 for broadcast)

---

## 5️⃣ Subnet Attributes Explained

| Attribute         | Description                            |
|------------------|----------------------------------------|
| Network Address   | All host bits = 0 (first IP, not usable) |
| Broadcast Address | All host bits = 1 (last IP, not usable) |
| First Usable      | Network Address + 1                   |
| Last Usable       | Broadcast Address – 1                 |
| Max Hosts         | `2^h – 2` (h = host bits)             |

---

### 🧪 Example: Subnet `192.168.10.0/26`

- **Subnet Mask**: 255.255.255.192  
- **Total IPs**: 64  
- **Network Address**: 192.168.10.0  
- **Broadcast Address**: 192.168.10.63  
- **First Usable**: 192.168.10.1  
- **Last Usable**: 192.168.10.62  
- **Max Hosts**: 62  

---

## 6️⃣ Special Subnet Cases

### 🔹 /30 — Point-to-Point Links
- 4 total IPs: 2 usable
- Used in router-to-router links

### 🔹 /31 — RFC 3021
- 2 total IPs: **both usable**
- No network/broadcast
- Efficient for P2P links (supported in Cisco IOS)

### 🔹 /32 — Host Route
- 1 IP address (single host)
- Used for:
  - Loopbacks
  - ACLs
  - Route summarization

---

## 7️⃣ VLSM Subnetting: Step-by-Step

Given: `192.168.1.0/24`

| Subnet | Required Hosts | CIDR | Assigned Block        | Usable IPs        |
|--------|----------------|------|------------------------|-------------------|
| A      | 50             | /26  | 192.168.1.0/26         | .1 – .62          |
| B      | 20             | /27  | 192.168.1.64/27        | .65 – .94         |
| C      | 10             | /28  | 192.168.1.96/28        | .97 – .110        |

> 🔁 Start with largest subnet and assign downwards.

---

## 8️⃣ Subnet Mask Summary Table

| CIDR | Subnet Mask       | Hosts/Subnet  |
|------|--------------------|----------------|
| /24  | 255.255.255.0     | 254            |
| /25  | 255.255.255.128   | 126            |
| /26  | 255.255.255.192   | 62             |
| /27  | 255.255.255.224   | 30             |
| /28  | 255.255.255.240   | 14             |
| /29  | 255.255.255.248   | 6              |
| /30  | 255.255.255.252   | 2              |
| /31  | 255.255.255.254   | 2 (P2P)        |
| /32  | 255.255.255.255   | 1 (Host)       |

---

## 🧠 Final Thoughts

Whether you're running an enterprise network or prepping for your CCNA exam, understanding **CIDR** and **subnetting** is foundational.

> 🏁 Classful addressing is gone — but its lessons helped shape the efficient, scalable networks we rely on today.

---

## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
