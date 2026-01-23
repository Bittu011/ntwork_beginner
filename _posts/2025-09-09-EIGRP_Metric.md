---
title: EIGRP Metric Calculations
date: 2025-09-09 04:00:00 +0000
categories: [Networking_Topics]
tags: [eigrp_metric, eigrp, ccna, ccnp, networking]
---


<h1 align="center">🧮 EIGRP Metric Calculation Understanding the “Best Path”</h1>

Enhanced Interior Gateway Routing Protocol (EIGRP) is fundamentally a **distance vector**
routing protocol, but it surpasses older protocols like RIP by using a **sophisticated,
multi-component metric system**. This system determines the *“best path”* in the network,
ensuring **rapid convergence** and **accurate path selection** across modern high-speed
infrastructures.

If you’re preparing for an interview or want a deep technical revision, mastering
EIGRP’s metric formula is essential — it’s the heart of the protocol’s routing logic.


<h2 align="center">📘 PART 1: THE BUILDING BLOCKS (VECTOR METRICS)</h2>


Unlike RIP, which relies solely on hop count, **EIGRP uses multiple vector metrics** to
calculate the desirability of a route. These are accumulated from the destination
back to the source.

EIGRP uses **five key vector metrics**:

| Metric Component | Description | Technical Details |
|------------------|--------------|-------------------|
| **Bandwidth (BW)** | Lowest configured bandwidth along the entire path | Expressed in **kbps**. Used as **inverse minimum bandwidth**, scaled by a factor of **10⁷** |
| **Delay (DLY)** | Cumulative transmission delay across all outgoing interfaces | Measured in **tens of microseconds (μs)** |
| **Reliability (REL)** | Indicates link quality (error rate) | Value between **1–255**, with **255/255 = 100% reliable** |
| **Load (LOAD)** | Indicates link utilization or congestion | Range **1–255**, where **255 = 100% saturation** |
| **MTU** | Minimum MTU size along the path | Not used in composite metric but **carried in vector** |

**Accumulation Rules:**
- **Bandwidth:** Minimum value across the path  
- **Delay:** Summation of all delays  
- **Reliability:** Minimum value  
- **Load:** Maximum value  
- **MTU:** Minimum value  
- **Hop Count:** Accumulative


<h2 align="center">⚙️ PART 2: THE CLASSIC METRIC FORMULA (32-BIT)</h2>


EIGRP’s composite metric is derived from IGRP’s formula, multiplied by **256**
to extend it from 24 bits to 32 bits.

### 📐 Full Formula:

>   Metric = 256 × [(K1 × BW + (K2 × BW) / (256 - Load) + K3 × Delay) × (K5 / (K4 + Reliability))]



**Where:**
- **BW** → Inverse minimum bandwidth (scaled to 10⁷)
- **Delay** → Total delay in tens of microseconds (μs)
- If **K5 = 0**, the reliability quotient is defined as **1**

### ⚖️ Default K Values:
| Coefficient | Purpose | Default Value |
|--------------|----------|----------------|
| **K1** | Bandwidth | 1 |
| **K2** | Load | 0 |
| **K3** | Delay | 1 |
| **K4** | Reliability | 0 |
| **K5** | Reliability | 0 |
| **K6** | Extended Attributes (Wide Metrics only) | 0 |

**Note:**  
All K-values **must match** between neighbors for adjacency formation and route exchange.

### 🧮 Simplified Default Formula:
When default K-values are applied:

>   Metric = 256 × ( (10⁷ / Minimum Bandwidth) + Total Delay )


- **Bandwidth** → in kilobits per second (kbps)
- **Delay** → in tens of microseconds (μs)

──────────────────────────────────────────────────────────────────────────────
🚀 PART 3: WHY WIDE METRICS WERE INTRODUCED
──────────────────────────────────────────────────────────────────────────────

Classic metrics had **scalability issues** on high-speed links (1G, 10G, etc.).
For instance, both GigabitEthernet and TenGigabitEthernet could result in the
same calculated metric — preventing accurate differentiation of link speeds.

**Wide Metrics** were introduced to address this issue by providing
**greater precision and scalability**.

### 🔧 Key Enhancements in Wide Metrics:
1. **Scaling Factor:**  
   - Classic: ×256  
   - Wide: ×65,535

2. **Delay Unit:**  
   - Classic: **microseconds (10⁻⁶)**  
   - Wide: **picoseconds (10⁻¹²)** — much finer granularity

3. **New Coefficient:**  
   - **K6** introduced for **Extended Attributes** (e.g., Jitter, Energy metrics)

### 📐 Wide Metric Formula:

>   Wide Metric = 65,535 × [(K1 × BW + (K2 × BW) / (256 - Load) + K3 × Latency + K6 × Extended) × (K5 / (K4+ Reliability))]


──────────────────────────────────────────────────────────────────────────────
🧭 PART 4: CONFIGURATION & KEY TAKEAWAYS
──────────────────────────────────────────────────────────────────────────────

### 🔸 Metric Styles:
| Configuration Mode | Metric Type Used |
|--------------------|------------------|
| Classic Mode | Classic Metric |
| Named Mode | Wide Metric |

### 🔸 Influencing EIGRP Path Selection:
You can modify EIGRP’s route decision process by tuning metric components:

1. **Interface Delay**  
   - Adjusting delay affects only EIGRP calculations (preferred method).  
   - Configured in **tens of microseconds**.  
   - Command Example:  
     ```
     Router(config-if)# delay <tens-of-microseconds>
     ```

2. **K-Values (Metric Weights)**  
   - Modify the influence of bandwidth, delay, load, or reliability.  
   - **K-values must match** between neighbors.  
   - Command Example:  
     ```
     Router(config-router)# metric weights 0 K1 K2 K3 K4 K5
     ```

3. **Offset Lists**  
   - Add a static offset to route metrics to make a path less desirable.  
   - Commonly used for **manual route preference tuning**.  
   - Command Example:  
     ```
     Router(config-router)# offset-list <access-list> in/out <offset-value> <interface>
     ```

──────────────────────────────────────────────────────────────────────────────
🏁 SUMMARY
──────────────────────────────────────────────────────────────────────────────

✅ **EIGRP Metric Components:** Bandwidth, Delay, Load, Reliability, MTU  
✅ **Default Calculation Uses:** Bandwidth + Delay  
✅ **Classic Metric:** 32-bit; scaled by ×256  
✅ **Wide Metric:** 64-bit-like precision; scaled by ×65,535  
✅ **Key Configuration Difference:** Classic → Classic Mode; Wide → Named Mode  
✅ **Influence Path Selection:** Adjust Delay or Offset Lists  
✅ **Successor Route:** The path with the **lowest composite metric**  
✅ **Supports:** Unequal-cost load balancing using the **variance** command

──────────────────────────────────────────────────────────────────────────────
🎯 FINAL THOUGHT:
──────────────────────────────────────────────────────────────────────────────
The **EIGRP metric formula** is the beating heart of EIGRP’s intelligence.
It not only determines the **successor** and **feasible successor routes** but
also enables **loop-free convergence** and **load balancing** in real time.
Understanding it gives you complete control over how EIGRP “thinks” when choosing
the best path in your network.
──────────────────────────────────────────────────────────────────────────────



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)

