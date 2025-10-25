---
title: BGP Attributes - Technical Notes
date: 2025-10-20 04:00:00 +0000
categories: [CCNA]
tags: [ccna, ntworkengineer, ccnp enarsi, ccnp, bgp]
---

# 📘 BGP Path Attributes: Comprehensive Technical Notes

> **Audience:** Interviewers • Network Engineers • Learners • Revision Notes  
> **Purpose:** A one-stop, technically complete, copyright-free guide to **BGP Path Attributes (PAs)** for understanding, reviewing, and mastering BGP path selection.

---

## 🧩 I. Prerequisites & Overview

Before diving into BGP Path Attributes, ensure the following concepts are clear:

- **BGP Complexity:**  
  Border Gateway Protocol (BGP) is among the most complex routing protocols used in networking.

- **Routing Basics:**  
  Understanding of IP packet routing and route advertisement.

- **BGP Peering Knowledge:**  
  Basic knowledge of **EBGP (External BGP)** and **IBGP (Internal BGP)** sessions.

- **IBGP vs. EBGP Behavior:**  
  - **EBGP Update:** Routes learned from an **external peer** can be sent to all other peers.  
  - **IBGP Update:** Routes learned from an **internal peer** can be advertised to **external peers**, but not to other **internal peers** (loop prevention rule).

---

## 🧠 II. What Are Path Attributes (PAs)?

Path Attributes are **parameters** within a **BGP Update message** that describe the characteristics of advertised routes.

- **Location:**  
  Found inside **BGP Update messages**, which carry new prefixes or withdraw old ones.

- **Structure:**  
  - **Prefixes (routes)** sit at the bottom of the update.
  - **Path Attributes** sit above them, defining properties of those routes.

- **Purpose:**  
  1. **Best Path Selection:** Helps BGP decide the most optimal route when multiple paths exist.  
  2. **Loop Prevention:** Detects if a route re-enters the same AS.  
  3. **Route Classification:** Tags routes for filtering, redistribution, or policy decisions (e.g., BGP Communities).  
  4. **Informative Use:** Certain attributes (e.g., *Atomic Aggregate*) signal route summarization.

---

## 🧱 III. Categories of Path Attributes

Each Path Attribute falls into one of four key categories:

| **Category** | **Well-Known/Optional** | **Mandatory/Discretionary** | **Transitivity** | **Description & Rule** | **Examples** |
|---------------|--------------------------|------------------------------|------------------|-------------------------|---------------|
| **Well-Known Mandatory** | Well-Known | Mandatory | Always | Must be recognized and included by every BGP device. | `Next Hop`, `AS Path`, `Origin` |
| **Well-Known Discretionary** | Well-Known | Discretionary | Always | Understood by all BGP routers but optional for use. | `Local Preference`, `Atomic Aggregate` |
| **Optional Transitive** | Optional | Discretionary | Transitive | May not be understood; if unknown, still passed along to other ASes. | `Aggregator`, `Community` |
| **Optional Non-Transitive** | Optional | Discretionary | Non-Transitive | May not be understood; **not** shared outside the AS. | `MED`, `Originator ID`, `Cluster List` |

---

## 🧮 IV. BGP Best Path Selection Process (BPSP)

BGP uses a **sequential decision algorithm** to determine the *best* route among multiple available ones.  
The steps below follow a common mnemonic: **N-W-L-O-A-O-M-N-I-O-R-I**.

| **#** | **Attribute / Rule** | **Preference** | **Key Details** |
|-------|----------------------|----------------|-----------------|
| 1️⃣ | **Next Hop Reachability (N)** | Must be reachable | If the next hop is unreachable, the route is unusable (remains in the BGP table but not in RIB). |
| 2️⃣ | **Weight (W)** | Highest wins | Cisco-proprietary; locally significant; not carried in updates (default 0). |
| 3️⃣ | **Local Preference (L)** | Highest wins | Influences routing inside the AS; default value 100; non-transitive. |
| 4️⃣ | **Locally Originated (O)** | Local routes preferred | Routes injected via `network` or `redistribute` commands are preferred. |
| 5️⃣ | **AS Path Length (A)** | Shortest wins | Fewer AS hops preferred; can be lengthened using **AS Path Prepending**. |
| 6️⃣ | **Origin Code (O)** | IGP > Incomplete | IGP (`i`) = from network command; Incomplete (`?`) = from redistribution. |
| 7️⃣ | **MED (M)** | Lowest wins | Suggests preferred entry point into AS; default 0; compared among same external AS. |
| 8️⃣ | **EBGP over IBGP (N/E)** | EBGP preferred | External paths take precedence over internal. |
| 9️⃣ | **IGP Metric (I)** | Lowest wins | Applies to IBGP-learned routes — smaller IGP cost to next hop wins. |
| 🔟 | **Oldest EBGP Route (O/D)** | Oldest wins | If multiple EBGP routes exist for the same prefix. |
| 11️⃣ | **BGP Router ID (R)** | Lowest wins | Used as a tie-breaker. |
| 12️⃣ | **Neighbor IP Address** | Lowest wins | Final tie-breaker to ensure deterministic selection. |

---

## ⚙️ V. Key Path Attributes & Manipulation

### 🔸 A. **Weight (Cisco Proprietary)**
- **Purpose:** Influences local path selection (highest wins).  
- **Scope:** Local to the router; not shared with peers.  
- **Example:**  
```bash
  route-map SET_WEIGHT permit 10
   set weight 200
  neighbor 10.1.1.2 route-map SET_WEIGHT in
```

###  🔸 B. **Local Preference**

-   **Purpose**: Controls routing within the AS (highest preferred).
-   **Scope**: Non-transitive (not advertised to other ASes).
-   **Default**: 100.
-   **Example**:
```bash
route-map SET_LOCAL_PREF permit 10
 set local-preference 200
neighbor 10.1.1.2 route-map SET_LOCAL_PREF in
```

### 🔸 **C. AS Path Prepending**

-   **Purpose**: Artificially lengthen the AS Path to make a route less preferred by external peers.
-   **Best Practice**: Only prepend your own AS number.
-   **Example**:
```bash
route-map PREPEND-AS permit 10
 set as-path prepend 65001 65001 65001
neighbor 203.0.113.1 route-map PREPEND-AS out
```


### 🔸 **D. Multi Exit Discriminator (MED)**

-   **Purpose**: Suggests preferred inbound path for external ASes (lower wins).
-   **Default**: 0.
-   **Comparison**: Only compared if routes come from the same external AS (can be overridden).
-   **Example**:
```bash
route-map SET_MED permit 10
 set metric 50
neighbor 203.0.113.2 route-map SET_MED out
```


### E. **Origin Code**

-   **Purpose**: Indicates how the route entered BGP.
-   **Values**:
    -   `i` – From the network command (IGP-originated).
    -   `?` – From redistribution (incomplete).
**Preference**: IGP-originated (`i`) preferred over incomplete (`?`).


### 🔸 F. **BGP Communities**

**Purpose**: Tag routes for policy decisions or filtering.
**Types**:
    -   **Standard (16-bit)**
    -   **Extended (32-bit)**
    

**Common Well-Known Communities**:

`no-advertise` → Do not advertise further.
`no-export` → Do not advertise outside local AS (IBGP only).
`local-AS` → Keep route within local AS.


## 🔁 VI. Other Essential Concepts


### 1. Route Policy Application

-   Updating route policies (e.g., route maps) does not immediately affect existing routes in the BGP table.

### 2. Route Refresh

-   Used to reapply new policies by refreshing route advertisements.

```bash
clear ip bgp * in     # Request inbound updates (refresh inbound)
clear ip bgp * out    # Resend outbound updates (refresh outbound)
```

### 3. Redistribution Caution

-   Avoid redistributing BGP routes into IGPs (like OSPF/EIGRP) due to scalability limits — BGP handles far larger routing tables than IGPs.


## 🧾 Summary: Key Takeaways

| **Concept**      | **Purpose**                        | **Scope**    | **Preference Rule** |
| ---------------- | ---------------------------------- | ------------ | ------------------- |
| Weight           | Local path choice                  | Local router | Highest wins        |
| Local Preference | AS-internal routing                | Within AS    | Highest wins        |
| AS Path          | Loop prevention / external routing | Inter-AS     | Shortest wins       |
| Origin Code      | Route source info                  | Inter-AS     | IGP > Incomplete    |
| MED              | External entry preference          | Between AS   | Lowest wins         |
| Community        | Route classification               | Local/Global | Policy dependent    |


## 🏁 Final Notes


-   BGP Path Attributes are **core to route decision-making** in modern networks.
-   Mastery of each attribute — its **type, scope, and preference order** — is essential for network design, troubleshooting, and optimization.
-   Understanding how PAs interact ensures **predictable, policy-driven routing behavior** across both internal and external BGP topologies.



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)

