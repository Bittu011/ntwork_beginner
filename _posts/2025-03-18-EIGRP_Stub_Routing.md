---
title: EIGRP Stub Routing
date: 2025-03-18 05:00:00 +0000
categories: [Networking_Topics]
tags: [networking, eigrp, networkengineer, cisco, itlab]
---

# 📘 EIGRP Stub Routing – Notes

## 🔹 What is EIGRP and Why Do We Need Stub Routers?
- **EIGRP (Enhanced Interior Gateway Routing Protocol)** is like a group of routers constantly chatting and sharing directions (routes).  
- Routers use **DUAL (Diffusing Update Algorithm)** to find the best paths. If a path fails, they send **queries** asking neighbors if they know another way.  
- Problem: Sending queries to routers that are **dead-ends** (like small offices or remote sites) wastes bandwidth and CPU, and may even cause **Stuck-in-Active (SIA)** events.  

👉 **Stub Routers solve this problem** by limiting EIGRP queries and advertisements.  

---

## 🔹 What is an EIGRP Stub Router?
- A **Stub Router** is a router that:  
  - Acts as a **non-transit router** (traffic doesn’t pass through it to reach other routers).  
  - **Doesn’t receive queries** from neighbors.  
- Think of it as the **quiet router** in the group — it only speaks when needed, and others know not to ask it complicated questions.  

---

## 🔹 Why Use EIGRP Stub Routing? (Key Benefits)
1. **Improved Performance**
   - Stops sending Hellos on interfaces with no EIGRP routers.  
   - Reduces unnecessary query traffic.  

2. **Enhanced Security**
   - Using **passive interfaces**, prevents rogue routers from forming neighborships.  

3. **Increased Stability**
   - Prevents **SIA (Stuck-in-Active)** events.  

4. **Faster Convergence**
   - Core routers don’t wait for responses from stub routers, so the network adapts quicker.  

---

## 🔹 How to Configure a Stub Router

### 1. Optional – Passive Interface  
Stops Hellos on interfaces with no EIGRP neighbors.  

```bash
router eigrp 1
passive-interface g0/1
```


> The network is still advertised, but no neighborship is formed.  

### 2. Enable Stub  

```bash
router eigrp 1
eigrp stub
```

> By default, this advertises **directly connected routes** and **summary routes**.  

### 3. Verify  

```bash
show ip eigrp neighbors detail
```

> Displays if the neighbor is a **stub** and what routes it advertises.  

---

## 🔹 Controlling Stub Advertisements
- `eigrp stub connected` → Only advertises directly connected networks.  
- `eigrp stub static` → Advertises static routes.  
- `eigrp stub redistributed` → Advertises redistributed routes.  
- `eigrp stub summary` → Advertises summary routes.  
- `eigrp stub receive-only` → Doesn’t advertise anything; only receives routes.  
- `eigrp stub leak-map <map-name>` → Selectively advertises routes using ACLs + route maps.  

---

## 🔹 How Neighbors Recognize a Stub
- The stub router adds a **TLV code** in its Hello messages.  
- Neighbors recognize it and **stop sending queries** to the stub.  

---

## 🔹 Analogy – Easy Way to Remember
- Imagine your network is an **office building**:  
  - **EIGRP query** = someone shouting: *“Where’s the stapler?”*  
  - **Normal router** = open office, where people may ask each other and forward the question.  
  - **Stub router** = a tiny filing room at the end of the hall. Nobody asks them because they only deal with their files and won’t forward the question.  

---

✅ **Key Takeaway:**  
Stub routers are best for **remote sites or edge routers**. They reduce unnecessary queries, improve security, prevent SIA events, and speed up convergence.  



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
