---
title: OSPF Metric
date: 2025-10-07 04:00:00 +0000
categories: [Networking_Topics]
tags: [ospf_metric ,ccna, ccnp, cisco, network_topics]
---

<h1 align="center">   📘 OSPF Cost – Complete Technical Guide for Interview & Learning</h1>

**Purpose**: This guide is designed for learners, professionals, and interviewers who want a complete and technical understanding of how **OSPF (Open Shortest Path First)** uses Cost as its routing metric. It includes all important concepts, formulas, configurations, and commands in a structured format for **learning, revision, and reference**.


<h2 align="center">🧠 1. What is OSPF Cost?</h2>


**Definition**:
OSPF Cost is the metric used by OSPF to determine the best (shortest) path to a destination network. **Lower cost = preferred path**.

**Key Points**:

- Every OSPF-enabled interface has an associated cost.

- OSPF uses **cumulative cost**: the total cost of all outgoing interfaces in a path.

- Cost and Metric are used interchangeably (Cost is more commonly used in OSPF).

**Example**:
If a path traverses three links with costs 1, 1, and 2, the total OSPF cost = 1 + 1 + 2 = 4.


<h2 align="center">📡 2. How OSPF Cost is Calculated</h2>


**Formula**:
```bash
OSPF Cost = Reference Bandwidth / Interface Bandwidth
```

🔢 **Default Reference Bandwidth: `100 Mbps`(100,000,000 bps)**

| Interface Bandwidth | OSPF Cost (Default)                  |
| ------------------- | ------------------------------------ |
| 10 Mbps             | 100 / 10 = 10                        |
| 100 Mbps            | 100 / 100 = 1                        |
| 1 Gbps              | 100 / 1000 = 0.1 → Rounded up to 1   |
| 10 Gbps             | 100 / 10000 = 0.01 → Rounded up to 1 |

> ⚠️ **Important**: OSPF cost values are always integers. Any value < 1 is rounded up to 1.


<h2 align="center">⚠️ 3. Suboptimal Routing with Default Costs</h2>

With the default 100 Mbps reference bandwidth:

- **1 Gbps and 10 Gbps** links get same cost as **100 Mbps: 1**

- Leads to **suboptimal path selection** in modern networks

- **Equal-Cost Multi-Path (ECMP)** issues: OSPF may balance across unequal links

### 🔄 ECMP Behavior:

- OSPF supports up to **4 equal-cost paths** by default

- If unequal links have the same OSPF cost (e.g., 100 Mbps and 10 Gbps), traffic is unevenly balanced.


<h2 align="center">🛠️ 4. Manipulating OSPF Cost (Traffic Engineering Techniques)</h2>

### 🔧 Method 1: Change Reference Bandwidth (***Recommended***)

Use this to make OSPF cost calculations more accurate for high-speed links.
```bash
R1(config-router)# auto-cost reference-bandwidth 10000
```

This sets the reference bandwidth to **10 Gbps**.

**Best Practice**:

- Set same reference bandwidth on **all routers** in the OSPF domain

- Choose a value **equal to or greater than** the fastest link

### ⚙️ Method 2: Manually Set OSPF Cost on Interface

Override cost calculation manually.

**Command**:

```bash
R1(config-if)# ip ospf cost 50
```

Use this when:

- You want to influence OSPF route selection manually

- Link speed does not reflect actual link desirability


### 🧮 Method 3: Change Interface Bandwidth Value


Affects the cost indirectly via the formula.

**Command**:

```bash
R1(config-if)# bandwidth 100000
```

**Note**:

- Bandwidth is set in Kbps

- Changing bandwidth affects QoS and other features

- Does not change actual physical speed


<h2 align="center">👀 5. Viewing OSPF Cost – Key Show Commands</h2>

| Command                                    | Purpose                                                     |
| ------------------------------------------ | ----------------------------------------------------------- |
| `show ip ospf interface brief`             | Quick overview: interface cost, state, area, neighbor count |
| `show ip ospf interface [type/number]`     | Detailed interface OSPF data, including exact cost          |
| `show ip ospf database router [router-id]` | Displays LSA Type 1 data and associated metrics in the LSDB |


<h2 align="center">🧬 6. Advanced: OSPF Two-Part Metric (RFC 8042)</h2>

**What**: OSPF extension allowing a **two-part metric** in special environments

**Structure**:

1.  **Router-to-Network Cost** (outbound)

2.  **Network-to-Router Cost** (inbound)

**Use Case**:

- Helps in **multi-access/broadcast** environments (e.g., satellite links)

- Reduces LSA update frequency

- Advertised via **Extended Link TLVs** with **Sub-TLVs** in OSPFv2

**Implementation Requirement**:

- All routers in the area must support this extension

<h2 align="center">📝 Summary & Key Takeaways</h2>

| Concept             | Description                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------- |
| OSPF Cost           | Metric for selecting shortest path (lower = better)                                         |
| Default Formula     | Reference Bandwidth (100 Mbps) / Interface Bandwidth                                        |
| Default Limitation  | Fails to differentiate between fast links (≥ 100 Mbps)                                      |
| Traffic Engineering | Use methods like changing reference bandwidth, manual cost setting, or bandwidth adjustment |
| Advanced Topics     | Two-part metrics for complex environments                                                   |
| Show Commands       | Essential for troubleshooting and verification                                              |


<h2 align="center">🎯 Pro Interview/Revision Tips</h2>

✅ Memorize the **default formula** and common OSPF costs

✅ Understand **why and how to manipulate OSPF costs**

✅ Be ready to **explain ECMP behavior and its drawbacks**

✅ Know **commands** to verify and troubleshoot OSPF cost

✅ Stay updated on **advanced OSPF enhancements** like RFC 8042

---
> **🧩 Understanding OSPF Cost is critical** for designing efficient networks and performing accurate traffic engineering. Misconfigured costs can lead to routing loops, suboptimal paths, and poor bandwidth utilization.



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
