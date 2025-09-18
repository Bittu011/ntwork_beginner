---
title: VTP & DTP
date: 2025-10-28 04:00:00 +0000
categories: [Networking_Topics]
tags: [vtp, dtp, ccna, ccnp, cisco, network_topics]
---

<h1 align="center">📘 Dynamic Trunking Protocol (DTP) & VLAN Trunking Protocol (VTP)</h1>
Complete Technical Notes for Learners, Interview Prep, and Revision


### 🔹 Introduction: Auxiliary VLAN Protocols
- **Dynamic Trunking Protocol (DTP)** and VLAN Trunking Protocol (VTP) are Cisco-proprietary protocols.

- **Purpose**:

    - **DTP** → Automates trunk establishment between switches.

    - **VTP** → Synchronizes VLAN database across switches.

- Neither carries user data frames; they support VLAN configuration & management.

- Though **removed from CCNA exam topics (2020)**, they remain expected knowledge in networking interviews and real-world practice.


<h2 align="center">1️⃣ Dynamic Trunking Protocol (DTP)</h2>

### 1.1 Definition & Purpose

- **Cisco-proprietary L2 protocol** to dynamically negotiate port mode (access/trunk) and encapsulation (802.1Q or ISL).

- **Goal**: Reduce manual VLAN trunk configuration.

- **Default**: Enabled on Cisco switches.

- **Limitation**: Works only on Cisco switches (manual config required for routers & non-Cisco gear).

### 1.2 Key Concepts

- **Administrative Mode** → Configured mode (switchport mode).

- **Operational Mode** → Actual working mode (access or trunk).

- **DTP Messages** → Sent to neighbors to negotiate mode.

- **Negotiation** → Based on admin modes, switches decide trunking.


### 1.3 DTP Switch Port Administrative Modes

| Mode                  | Behavior            | Key Points                                                       |
| --------------------- | ------------------- | ---------------------------------------------------------------- |
| **access**            | Fixed as access     | No DTP, permanent non-trunk                                      |
| **trunk**             | Fixed as trunk      | Sends DTP, ensures neighbor trunks                               |
| **dynamic auto**      | Passive negotiation | Forms trunk only if other side is trunk/desirable (default mode) |
| **dynamic desirable** | Active negotiation  | Actively forms trunk if possible                                 |
| **nonegotiate**       | DTP disabled        | Must manually configure trunking                                 |


### 1.4 DTP Negotiation Matrix

| Admin Modes   | access    | trunk     | desirable | auto   |
| ------------- | --------- | --------- | --------- | ------ |
| **access**    | access    | ❌ invalid | access    | access |
| **trunk**     | ❌ invalid | trunk     | trunk     | trunk  |
| **desirable** | access    | trunk     | trunk     | trunk  |
| **auto**      | access    | trunk     | trunk     | access |

> ⚠️ **access + trunk = invalid** → causes mismatch/blocking.


### 1.5 Trunk Encapsulation Negotiation

- Encapsulation types: 802.1Q (modern standard) & ISL (legacy).

- Defaults:

    - `switchport trunk encapsulation negotiate` (old).

    - Best Practice: Manually set `switchport trunk encapsulation dot1q`.


### 1.6 Security Concerns

- **VLAN Hopping Attack**:
Attackers use tools (e.g., Yersinia) to send fake DTP messages and trick the switch into creating a trunk, gaining access to **all VLANs**.

- **Mitigation**:

    - `switchport mode access` (forces access mode).

    - `switchport nonegotiate` (disables DTP explicitly).


### 1.7 Commands

```bash
show interfaces <int> switchport   # View DTP status & mode
switchport mode {access | trunk | dynamic auto | dynamic desirable}
switchport trunk encapsulation dot1q
switchport nonegotiate
```

### 1.8 Advantages

✅ Automatic negotiation
✅ Reduces manual errors
✅ Adapts dynamically
✅ Scales for larger networks

### 1.9 Disadvantages

❌ Security vulnerabilities (VLAN hopping)
❌ Cisco-only
❌ Extra negotiation traffic
❌ Can cause unintended trunks


<h2 align="center">2️⃣ VLAN Trunking Protocol (VTP)</h2>

### 2.1 Definition & Purpose

- Cisco protocol for **centralized VLAN management**.

- Distributes **VLAN database** (vlan.dat) across switches.

- Saves admin effort, ensures consistency.


### 2.2 Key Concepts

- **VLAN Database**: Stored as `vlan.dat`.

- **VTP Messages**: Sent via trunk ports (dest. MAC `01-00-0C-CC-CC-CC`).

- **Revision Number**: Incremented on changes; higher rev overwrites lower.

- **Domain Name**: Must match across switches.

- **Password**: Optional, used for authentication.


### 2.3 VTP Message Types

- **Summary Advertisements** → Sent every 5 mins (domain + revision).

- **Subset Advertisements** → Sent after VLAN changes.

- **Advertisement Requests** → Sent by new/reset switch.


### 2.4 VTP Modes

| Mode                 | Behavior                                                    |
| -------------------- | ----------------------------------------------------------- |
| **Server (default)** | Create/delete VLANs, sync changes, stored in NVRAM.         |
| **Client**           | Cannot create VLANs, syncs from server, not saved in NVRAM. |
| **Transparent**      | Local VLAN changes only, forwards VTP msgs, rev# = 0.       |
| **Off (VTPv3)**      | Local VLAN changes only, does not forward or sync.          |


### 2.5 VTP Versions

- **V1/V2**: Only normal VLANs (1–1005). Extended VLANs require transparent mode.

- **V3 (recommended)**:

    - Supports extended VLANs (1006–4094).

    - Introduces **Primary Server** role.

    - Introduces **Off mode**.

    - Prevents **VTP Bomb**.

### 2.6 Security: The "VTP Bomb"

- **Risk (VTPv1/2)**: A switch with higher revision overwrites VLAN database, wiping VLANs.

- **Mitigation**: Reset revision to 0 before connecting new switches.

- **VTPv3 Solution**: Only **Primary Server** can modify VLANs → prevents VTP Bomb.


### 2.7 Configuration Guidelines

- Keep **domain name, version, password** consistent.

- Reset revision before adding new switches.

- Ensure VLANs exist on servers before mode change.

- Trunk negotiation fails across different VTP domains → use manual trunk config.


### 2.8 VTP Pruning

- **Purpose**: Reduce unnecessary VLAN traffic across trunks.

- **Enabled on server** → applies to domain.

- **Ineligible VLANs**: VLAN 1, VLANs 1002–1005, extended VLANs.


### 2.9 Commands

```bash
vtp domain <name>         # Set domain
vtp mode {server | client | transparent | off}
vtp version {1 | 2 | 3}
vtp password <password>
vtp primary               # Set primary server (VTPv3)
show vtp status           # Check version, mode, revision, domain
show vtp counters
show vlan brief
```


### 2.10 Advantages

- ✅ Centralized VLAN management
- ✅ Reduces errors & admin workload
- ✅ Easy VLAN propagation
- ✅ Supports large campus networks


### 2.11 Disadvantages

- ❌ VTP Bomb risk (V1/V2)
- ❌ Cisco-only
- ❌ Careful revision management needed
- ❌ Broadcast traffic without pruning
- ❌ Large STP domain risks



<h2 align="center">📌 Quick Revision Cheatsheet</h2>

### 🔹 DTP

- Negotiates **access/trunk mode**.

- **Modes**: access, trunk, dynamic auto, dynamic desirable, nonegotiate.

- **Risk**: VLAN hopping → disable with `nonegotiate`.

### 🔹 VTP

- Syncs **VLAN database** across switches.

- **Modes**: server, client, transparent, off (V3).

- **Risk**: VTP Bomb (revision overwrite).

- **Fix**: Use VTPv3 + Primary Server.

- **Extra**: VTP Pruning saves `bandwidth`.



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
