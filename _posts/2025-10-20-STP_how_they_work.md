---
title: STP_How_They_Select
date: 2025-10-20 05:00:00 +0000
categories: [CCNA]
tags: [networking, STP , networkengineer, ccna, networkbasics]
---


# 🧵 **Spanning Tree Protocol (STP) — Complete Technical Notes**

A Layer-2 Ethernet network with redundant physical links prevents outages but introduces a major risk: **switching loops**. When loops occur, **BUM traffic** (Broadcast, Unknown unicast, Multicast) can circle endlessly, causing **broadcast storms**, MAC table instability, and network collapse.

**Spanning Tree Protocol (STP)** prevents loops by building a **loop-free logical topology**. It keeps some ports active and intentionally blocks others to maintain a single best path through the network.

---

# 🟦 **1. STP Core Concept**

- Redundant links = good for redundancy  
- But redundant paths = potential L2 loops  
- STP uses **BPDUs (Bridge Protocol Data Units)** to identify redundant paths  
- STP then:
  - Selects a **Root Bridge**
  - Selects **Root Ports** (one per non-root switch)
  - Selects **Designated Ports** (one per network segment)
  - Places all other ports in **Blocking** state

---

# 🟦 **2. STP Decision-Making Algorithm (3 Major Steps)**

## 🔹 **Step 1: Root Bridge Election**
STP first chooses one switch to act as the **Root Bridge** (the reference point for the entire STP topology).

### **How the Root Bridge is chosen**
Each switch advertises its **Bridge ID (BID)** in BPDUs.

**Bridge ID (64 bits) = Priority (16 bits) + MAC Address (48 bits)**

- Lower BID = higher priority in election  
- Default priority = **32768**
- If priorities tie → **lower MAC address** wins

### **Important Behavior**
- Every switch initially believes **it** is the root
- Election stabilizes once BPDUs converge

### **PVST+ Note**
Cisco networks typically use **PVST+ / Rapid PVST+**  
➡ One STP instance *per VLAN*  
➡ Allows different VLANs to have *different* Root Bridges  
➡ Enables **load balancing**

---

## 🔹 **Step 2: Root Port (RP) Selection**
Every **non-root switch** chooses exactly **one Root Port (RP)**.

The **Root Port** is the port with the **lowest-cost path** back to the Root Bridge.

### **Root Port Selection Criteria (strict order)**

1. **Lowest total Root Path Cost**
   - Link speeds → Cost values (examples):
     - 10 Mbps = 100  
     - 100 Mbps = 19  
     - 1 Gbps = 4  
     - 10 Gbps = 2  

2. **Lowest upstream Bridge ID**
3. **Lowest upstream Port ID**

➡ Only **ONE** RP per switch  
➡ RP is always in **Forwarding** state

---

## 🔹 **Step 3: Designated Port Selection & Blocking**

### **Designated Ports (DP)**
Each network segment (collision domain / link) must have exactly **one Designated Port**.

Rules:
- **All ports on the Root Bridge** are automatically Designated Ports  
- A port facing each switch’s Root Port becomes DP on the opposite side  
- On shared segments, the DP is chosen using:
  1. Lowest Root Path Cost  
  2. Lowest Bridge ID  

### **Non-Designated Ports (Blocking)**
Ports that are **neither RP nor DP** must block.

**Blocking means:**
- ❌ No frame forwarding  
- ❌ No MAC learning  
- ✔ Yes → BPDU reception (so topology changes can be detected)

Blocking ensures that redundant links exist physically but are not active logically—preventing loops.

---

# 🟦 **3. STP Port States (Original 802.1D)**

A port transitions through multiple states before becoming active. This prevents temporary loops during recalculation.

| **State**     | **Role**             | **Forwards Frames?** | **Learns MACs?** | **Time** |
|---------------|----------------------|------------------------|-------------------|----------|
| Blocking      | Non-Designated       | ❌ No                 | ❌ No             | Stable   |
| Listening     | Transitional         | ❌ No                 | ❌ No             | 15 sec   |
| Learning      | Transitional         | ❌ No                 | ✔ Yes            | 15 sec   |
| Forwarding    | Root or Designated   | ✔ Yes                | ✔ Yes            | Stable   |

➡ A newly active port takes **30 seconds** (15 + 15) before forwarding frames.

---

# 🟦 **4. Faster Convergence Mechanisms**

## ⚡ **Rapid Spanning Tree Protocol (RSTP / 802.1w)**
- Replaces slow timers with handshake-based synchronization  
- Converges in **1–3 seconds**  
- Introduces roles like:
  - Alternate Port  
  - Backup Port  

Cisco variant: **Rapid PVST+** (per-VLAN rapid STP)

---

## ⚡ **PortFast**
Used on **end-host ports**.

- Skips Listening & Learning  
- Immediately transitions to **Forwarding**  
- Should NOT be used on switch-to-switch links  
- Must be combined with **BPDU Guard**  
  - If a PortFast port receives a BPDU → port is disabled (err-disabled)
  - Protects from accidental loops or rogue switches

---

# 🟦 **5. Summary of STP Logic Flow**

1. Elect Root Bridge  
2. Choose one Root Port per non-root switch  
3. Choose one Designated Port per segment  
4. Everything else → Blocking  
5. STP builds a **loop-free**, **redundancy-aware**, **self-healing** Layer-2 topology  

If a link fails:  
- STP recalculates  
- Blocked ports may transition to forwarding  
- Connectivity is restored without creating loops

---

# 🟩 **Use Cases in Interviews**
- Explain Layer 2 loop prevention  
- Describe bridge ID, root election  
- Compare STP vs RSTP  
- Discuss PVST+ advantages  
- Explain troubles with misconfigured PortFast  
- Calculate STP path cost in topology diagrams  

---

# 🟩 **Real-World Advantages**
- Prevents broadcast storms  
- Enables physical redundancy safely  
- Supports VLAN-specific design (PVST+)  
- Provides automatic failover  
- Essential for enterprise switching networks  

---

# 🧾 **End of Notes – Full STP Coverage**
This document is rephrased, original, and safe for public website use.


## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
