---
title: EIGRP Path Selection
date: 2025-09-01 04:00:00 +0000
categories: [Networking_Topics]
tags: [eigrp_path, eigrp, ccna, ccnp, networking]
---


# EIGRP Path Selection: How DUAL Chooses the Best Route (and its Backups)

Enhanced Interior Gateway Routing Protocol (EIGRP) is renowned for its **fast convergence**, powered by the **Diffusing Update Algorithm (DUAL)**. Understanding how EIGRP selects primary and backup paths is essential for learners, interview preparation, and network engineers aiming to optimize path selection and stability.

---

## Part 1: The EIGRP Topology Table — The Brains of Path Selection

Unlike traditional distance-vector protocols, **EIGRP maintains a Topology Table**. This table contains all known destination networks within the EIGRP Autonomous System (AS) and tracks metrics received from neighbors.

| Term               | Technical Definition (The “Why”) |
|-------------------|----------------------------------|
| **Reported Distance (RD)** | Metric advertised by a neighbor for a specific destination. Represents the neighbor’s calculated best metric to reach that destination. |
| **Feasible Distance (FD)** | The metric for the path with the **lowest overall cost** to a destination, calculated locally. Records the smallest known metric since the route last entered Passive state. |
| **Successor Route** | The route with the **lowest path metric** to reach a destination. It is the **primary path** for forwarding traffic. |
| **Successor**      | The first next-hop router for the Successor Route. |

**Goal:** Calculate FD for every known network and identify the **best next-hop (Successor)**.

---

## Part 2: The Logic of Loop-Freedom — The Feasibility Condition (FC)

EIGRP ensures **loop-free backup paths** using the **Feasibility Condition (FC)**.  

**Feasibility Condition Rule:**  
A neighboring router qualifies as a **Feasible Successor (FS)** if:

>   Reported Distance (RD) < Feasible Distance (FD)


**Why RD < FD guarantees loop-freedom:**
- The neighbor offering the backup path is a **downstream router**.
- Its metric is smaller than the local router's historical best metric, so traffic moves closer to the destination.
- Routes meeting FC are **guaranteed loop-free** and eligible as backup paths.
- Neighbors not satisfying FC may be upstream and are **not used as backups** but are stored in the Topology Table.

---

## Part 3: Path Selection and Convergence in Action

EIGRP uses DUAL states to maintain stability and enable **fast convergence**:

### Passive State (Stable)
- Route is stable and in **Passive (P)** state if the Successor meets FC.
- If the primary path fails, the router checks for **Feasible Successors**.
- If a FS exists, the route remains Passive and switches immediately to FS — **fastest convergence**.

### Active State (Recalculation)
- Occurs when the primary path fails and **no Feasible Successor** is available.
- The router enters **Active (A)** state, sending **QUERY packets** to neighbors to find a loop-free path.
- If a route stays Active beyond the **Active Timer** (default 3 minutes), it is declared **Stuck in Active (SIA)**.
- Using FC minimizes Active state occurrences, improving **network stability and speed**.

---

## Part 4: Beyond the Best Path — Load Balancing

EIGRP provides flexible load balancing options:

### 1. Equal-Cost Multipathing (ECMP)
- Multiple successor routes with **identical metrics** can be installed in the **Routing Information Base (RIB)**.
- Default maximum paths: **4**, adjustable via `maximum-paths` command.

### 2. Unequal-Cost Load Balancing
- Feasible Successors with different metrics can be used safely.
- Achieved with the **variance multiplier**:

>   Install FS if FD <= Successor FD × variance



- FS paths are **already loop-free**, so unequal-cost load balancing is safe.

---

### Key Takeaways:
1. EIGRP uses **DUAL** to calculate **Feasible Distance (FD)** and choose **Successors**.
2. **Feasible Condition (FC)** ensures **loop-free backups**.
3. Passive/Active states control **fast convergence**.
4. Supports **ECMP and unequal-cost load balancing** via variance.
5. Topology Table stores all routes, including upstream routes, ensuring **efficient path selection**.

---

> **Pro Tip:** Understanding RD, FD, Successors, FS, and FC is crucial for both **network troubleshooting** and **ENARSI exam preparation**.



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)

