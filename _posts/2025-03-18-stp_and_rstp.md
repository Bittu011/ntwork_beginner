---
title: STP & RSTP
date: 2025-03-18 04:00:00 +0000
categories: [CCNA]
tags: [stp, rstp, ccna, ccnp, cisco, network_topics]
---


<h1 align="center">🌐 Spanning Tree Protocol (STP) & Rapid Spanning Tree Protocol (RSTP) — Technical Notes</h1>


<h2 align="center">📌 Introduction: Why STP is Critical</h2>

- Ethernet LANs use **redundant paths** for fault tolerance.

- **Layer 2 Loops** can occur due to lack of TTL (Time-to-Live) in Ethernet frames.

- Loops cause:
    - **Broadcast Storms** (uncontrolled BUM traffic)

    - **MAC Address Flapping** (instability in switch MAC tables)

- **STP (802.1D)** and **RSTP (802.1w)** solve these by:

    - Creating a **loop-free logical topology**

    - **Blocking redundant links**

<h2 align="center">🔧 STP (Spanning Tree Protocol) - IEEE 802.1D / Cisco PVST+</h2>

### 🔸 STP Overview

- Default protocol on Cisco switches.

- **PVST+**: Cisco’s version — one STP instance per VLAN (Per-VLAN STP).

- Operates via **BPDU (Bridge Protocol Data Unit)** exchanges every 2 seconds.


<h2 align="center">🔹 STP Algorithm: 3-Step Process</h2>


### 1. Root Bridge Election
Switch with the **lowest Bridge ID (BID)** becomes Root Bridge.

**BID** = Bridge Priority (default: 32768 + VLAN ID) + MAC Address.

**Commands**:
```bash
spanning-tree vlan 10 priority 0
spanning-tree vlan 10 root primary
```

- Verify: `show spanning-tree`

### 2. Root Port Selection

- On non-root switches, select port with lowest path cost to the root.

- Tie-breakers (in order):

    - **Lowest Root Path Cost**

    - **Lowest Neighbor BID**

    - **Lowest Neighbor Port ID**

    - **Lowest Local Port ID**

- Cost values:

    - 10 Mbps: 100

    - 100 Mbps: 19

    - 1 Gbps: 4

    - 10 Gbps: 2

### 3. Designated Port Selection

-  One **Designated Port per segment** (responsible for forwarding).

- All other ports: **Non-Designated (Blocking)**


<h2 align="center">🔹 STP Port States</h2>

1. **Disabled – Admin down**

2. **Blocking – No data traffic, receives BPDUs**

3. **Listening – No traffic, sending/receiving BPDUs**

4. **Learning – Learns MACs, no data forwarding**

5. **Forwarding – Full forwarding**

> ➡️ Transition: **Listening (15s) → Learning (15s) → Forwarding**


<h2 align="center">🔹 STP Timers</h2>


| Timer         | Default | Purpose                      |
| ------------- | ------- | ---------------------------- |
| Hello         | 2s      | BPDU interval                |
| Forward Delay | 15s     | Time in Listening & Learning |
| Max Age       | 20s     | Timeout for BPDU absence     |

> ➡️ Convergence Time: Up to 50 seconds


<h1 align="center">⚡ RSTP (Rapid STP) - IEEE 802.1w / Cisco Rapid PVST+</h1>

<h2 align="center">🔸 Key Improvements Over STP</h2>

| Feature                   | STP                              | RSTP                                 |
| ------------------------- | -------------------------------- | ------------------------------------ |
| Convergence Speed         | Up to 50s                        | < 1s (best case)                     |
| Port States               | 5 (including disabled)           | 3 (Discarding, Learning, Forwarding) |
| Transition Method         | Timer-based                      | Sync mechanism                       |
| Port Roles                | Root, Designated, Non-designated | Root, Designated, Alternate, Backup  |
| BPDU Behavior             | Only Root sends                  | All switches send BPDUs              |
| Topology Change Detection | Max Age (20s)                    | 3 missed BPDUs (6s)                  |


<h2 align="center">🔹 RSTP Port States</h2>

- **Discarding = Blocking + Listening**

- **Learning**

- **Forwarding**


<h2 align="center">🔹 RSTP Port Roles</h2>


| Role       | Purpose                                   |
| ---------- | ----------------------------------------- |
| Root       | Best path to root bridge                  |
| Designated | One per segment, forwards traffic         |
| Alternate  | Backup path to root (on different switch) |
| Backup     | Backup path on same segment (same switch) |


<h2 align="center">🔹 RSTP Link Types</h2>


| Type           | Description                 | Behavior                      |
| -------------- | --------------------------- | ----------------------------- |
| Point-to-Point | Full-duplex links (default) | Fast transitions              |
| Shared         | Half-duplex/hub links       | Falls back to STP behavior    |
| Edge           | End-device ports            | Instant forwarding (PortFast) |


- Configure with:

```bash
spanning-tree portfast
spanning-tree link-type point-to-point
```


<h2 align="center">🔹 RSTP Path Costs</h2>

RSTP uses both short and long path costs:

| Speed    | Short Cost | Long Cost |
| -------- | ---------- | --------- |
| 10 Mbps  | 100        | 2,000,000 |
| 100 Mbps | 19         | 200,000   |
| 1 Gbps   | 4          | 20,000    |
| 10 Gbps  | 2          | 2,000     |
| 100 Gbps | —          | 200       |
| 1 Tbps   | —          | 20        |
| 10 Tbps  | —          | 2         |


**Configure**:

```bash
spanning-tree pathcost method {short | long}
```

<h2 align="center">🛠️ Cisco STP Toolkit Features (STP & RSTP Enhancements)</h2>

| Feature         | Function                                                |
| --------------- | ------------------------------------------------------- |
| **PortFast**    | Enables fast transition on edge ports (end hosts only). |
| **BPDU Guard**  | Shuts down a PortFast port if BPDU is received.         |
| **Root Guard**  | Prevents external switches from becoming root.          |
| **Loop Guard**  | Prevents non-designated ports from errantly forwarding. |
| **BPDU Filter** | Stops BPDUs on specific ports (risky if misused).       |


<h2 align="center">🔀 MSTP (Multiple Spanning Tree Protocol) - IEEE 802.1s</h2>

- Solves scalability issues in PVST+ by grouping VLANs into MST instances.

- Runs a single STP instance per group (not per VLAN).

- Uses RSTP mechanics for fast convergence.

- Example: VLANs 1–50 → MSTI1, VLANs 51–100 → MSTI2


<h2 align="center">🌐 ICCP STP Application (RFC 7727)</h2>

- Used in Provider Edge (PE) redundancy scenarios.

- Multiple PEs act as a virtual root bridge.

- Uses ICCP TLVs for config/state sync:

    - STP Connect, Root Time, Region Config, etc.

- Enables active-active operation, unlike VPLS (active-standby).

- If a PE fails → remaining PEs recalculate and reconverge.


<h2 align="center">📡 RSTP Management via MIB – RFC 4318</h2>

- Adds managed objects to Bridge MIB (RFC 4188).

- Important Objects:

    - `dot1dStpVersion`

    - `dot1dStpPortAdminEdgePort`

    - `dot1dStpPortOperEdgePort`

    - `dot1dStpPortAdminPathCost`


- **Security Consideration**: SNMPv3 recommended due to sensitivity of STP control.


<h2 align="center">🔁 STP vs RSTP — Summary Table</h2>


| Feature                   | STP (802.1D / PVST+)             | RSTP (802.1w / Rapid PVST+)         |
| ------------------------- | -------------------------------- | ----------------------------------- |
| Convergence Time          | Up to 50s                        | < 6s (often < 1s)                   |
| Port States               | 5                                | 3                                   |
| Port Roles                | Root, Designated, Non-Designated | Root, Designated, Alternate, Backup |
| BPDU Handling             | Only root bridge generates       | All switches generate               |
| Topology Change Detection | 20s (Max Age)                    | 6s (3 missed BPDUs)                 |
| Edge Port Behavior        | Triggers TC                      | Does not trigger TC                 |
| Link Types                | N/A                              | P2P, Shared, Edge                   |
| PortFast/Toolkit Support  | Yes                              | Yes                                 |
| VLAN Scaling              | One STP per VLAN (PVST+)         | Better with MSTP                    |


<h2 align="center">✅ Interview Tips</h2>

- Be clear on Root Port vs Designated Port logic.

- Understand BPDU Guard vs Root Guard vs Loop Guard differences.

- Know the transition states and timers.

- Practice troubleshooting scenarios with:

```bash
show spanning-tree
debug spanning-tree events
```



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
