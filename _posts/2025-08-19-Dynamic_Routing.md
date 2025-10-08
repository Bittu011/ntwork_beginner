---
title: Dynamic Routing
date: 2025-08-19 04:00:00 +0000
categories: [Networking_Topics]
tags: [dynamic_routing, ccna, ccnp, networking]
---


<h1 align="center">📘 Dynamic Routing Protocols & Route Selection</h1>


> A Complete Technical Guide for Learners, Interviewees, and Network Engineers


<h2 align="center">🔹 Part 1: Dynamic Routing Basics</h2>


**What is Dynamic Routing?**

Dynamic routing allows routers to automatically exchange routing information using protocols, keeping their routing tables updated without manual configuration.

**🔄 Dynamic Routing vs. Static Routing**
| Feature                         | Static Routing           | Dynamic Routing                        |
| ------------------------------- | ------------------------ | -------------------------------------- |
| **Configured by**               | Manually by admin        | Automatically by protocol              |
| **Reaction to network changes** | No automatic response    | Adapts dynamically                     |
| **Scalability**                 | Limited and error-prone  | Scales to large networks               |
| **Use case**                    | Small or stable networks | Large, complex, or changing topologies |


> ✅ A router can use both static and dynamic routing simultaneously.


<h2 align="center">🔹 Part 2: Types of Dynamic Routing Protocols</h2>


**1️⃣ Interior Gateway Protocols (IGPs)**

Used **within** a single Autonomous System (AS).

**▶ By Algorithm:**


| Algorithm           | Description                                                                       | Protocols   |
| ------------------- | --------------------------------------------------------------------------------- | ----------- |
| **Distance-Vector** | Routers exchange known routes and hop counts without a complete network map.      | RIP, EIGRP  |
| **Link-State**      | Routers share link info, build a full network map (LSDB), and compute best paths. | OSPF, IS-IS |


**2️⃣ Exterior Gateway Protocols (EGPs)**


Used between Autonomous Systems (e.g., enterprise to ISP).

| Algorithm       | Description                                        | Protocol |
| --------------- | -------------------------------------------------- | -------- |
| **Path-Vector** | Selects routes based on AS-paths (AS-to-AS logic). | BGP      |


<h2 align="center">🔹 Part 3: Understanding Route Selection</h2>


<h3 align="center">2 Core Meanings of "Route Selection":</h3>

1.  **Routing Table Population**:
    Choosing which route(s) to insert into the routing table (only the best per destination is kept).

2.  **Packet Forwarding Decision**:
    Using the routing table to decide how to forward individual packets.


<h2 align="center">🔹 Part 4: Routing Table Population Logic</h2>

When multiple routes to the same destination exist, routers use:

<h3 align="center">1️⃣ Administrative Distance (AD)</h3>

Determines the trustworthiness of a source protocol.

| Route Source       | AD (Lower = Better) |
| ------------------ | ------------------- |
| Directly Connected | 0                   |
| Static Route       | 1                   |
| External BGP       | 20                  |
| EIGRP              | 90                  |
| OSPF               | 110                 |
| IS-IS              | 115                 |
| RIP                | 120                 |
| Internal BGP       | 200                 |
| Unusable           | 255                 |

> ✅ AD is only compared when multiple protocols provide routes to the same destination.

<h3 align="center">2️⃣ Metric</h3>

Used when multiple routes from the same protocol exist.

| Protocol  | Metric Type | Description                                      |
| --------- | ----------- | ------------------------------------------------ |
| **RIP**   | Hop Count   | Fewer hops = better                              |
| **OSPF**  | Cost        | Lower bandwidth = higher cost                    |
| **EIGRP** | Composite   | Uses bandwidth, delay (and others if configured) |


> 👉 **Lower metric is always preferred**.


<h3 align="center">3️⃣ Equal-Cost Multi-Path (ECMP)</h3>


If multiple routes to the same destination from the same protocol have equal metrics, they are all installed in the routing table for load balancing.


<h3 align="center">4️⃣ Floating Static Routes</h3>


A static route with higher AD than a dynamic route (e.g., AD 111 vs OSPF AD 110) is used as a backup, inserted only if the preferred route fails.


<h3 align="center">🔹 Part 5: Packet Forwarding Logic</h3>


After routing table population:
-   🧭 **Forwarding Rule**: Choose the most specific route (longest prefix match) for the destination IP.
-   ❌ **AD and Metric are NOT used** during packet forwarding.
-   ✅ Example:
    -   Destination: 10.0.1.5
    -   Match:
        -   10.0.0.0/8 (Generic)
        -   10.0.1.0/24 ✅ (More specific → Used)


<h3 align="center">🔹 Part 6: Activating Routing Protocols</h3>


**📥 Enabling Routing on Interfaces (OSPF Example)**

```bash
router ospf 1
 network 192.168.1.0 0.0.0.255 area 0
```


**🔍 Understanding Wildcard Masks**

Wildcard mask ≠ Subnet mask
Wildcard mask inverts the subnet mask.

| Subnet Mask           | Wildcard Mask |
| --------------------- | ------------- |
| 255.255.255.0 (/24)   | 0.0.0.255     |
| 255.255.255.252 (/30) | 0.0.0.3       |



**Binary Logic:**

-   `0` = bit must match
-   `1` = bit can vary


**🎯 OSPF `network` Command Tasks:**

1.  **Interface Matching**: Finds interfaces with IPs matching the `network` + `wildcard`.
2.  **OSPF Activation**: Enables OSPF on those interfaces.
3.  **Prefix Advertisement**: Router advertises the actual interface subnet, not the network command range.


**🧠 Pro Tip:**

To control precisely which interface runs OSPF:

```bash
network 192.168.1.1 0.0.0.0 area 0
```

This ensures only that interface is affected.


<h2 align="center">📝 Final Notes for Interview & Exam Readiness</h2>

**✅ Know All AD Values**
**✅ Compare Metric Logic of RIP, OSPF, and EIGRP**
**✅ Understand ECMP & Floating Static Routes**
**✅ Be clear on the difference between Routing Table Population and Packet Forwarding**
**✅ Master OSPF network command & wildcard mask behavior**


<h2 align="center"></h2>


-   In enterprise networks, OSPF and EIGRP dominate for internal routing.
-   BGP is essential for Internet routing and multi-ISP connections.
-   Floating static routes are key for backup WAN paths.
-   Always test routing configurations using tools like:
    -   `show ip route`
    -   `show ip protocols`
    -   `show ip protocols`


<h2 align="center">✅ Summary Checklist</h2>

| Concept                               | Must Know? |
| ------------------------------------- | ---------- |
| Difference: Static vs Dynamic Routing | ✅          |
| Types of Routing Protocols (IGP, EGP) | ✅          |
| AD & Metric Logic                     | ✅          |
| ECMP                                  | ✅          |
| Floating Static Routes                | ✅          |
| Longest Prefix Match (Forwarding)     | ✅          |
| OSPF Network Command & Wildcard       | ✅          |



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
