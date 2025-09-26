---
title: CDP & LLDP
date: 2025-06-10 04:00:00 +0000
categories: [Networking_Topics]
tags: [cdp, lldp, ccna, ccnp, cisco, network_topics]
---


<h1 align="center">📘 Comprehensive Notes: Cisco Discovery Protocol (CDP) & Link Layer Discovery Protocol (LLDP)</h1>

<h2 align="center">1. 🔹 Introduction to Layer 2 Discovery Protocols</h2>

- In complex networks, maintaining accurate documentation is challenging. 
- Layer 2 discovery protocols simplify this by enabling devices to share information with directly connected neighbors.

**Purpose**:

- Automatically discover network topology.

- Identify devices, models, interfaces, IP addresses, and more.

**Key Protocols**:

- **CDP (Cisco Discovery Protocol)**: Cisco-proprietary.

- **LLDP (Link Layer Discovery Protocol)**: IEEE 802.1AB vendor-neutral standard.

These protocols also assist with **Detecting Network Attachment (DNA)** for rapid configuration change detection.


<h2 align="center">2. 🔹 Cisco Discovery Protocol (CDP)</h2>

### 2.1 Overview & Operation

- Proprietary to Cisco.

- Runs at Layer 2 (Data Link Layer).

- Enabled by default on Cisco devices.

- Sends advertisements periodically to neighbors.

- **Multicast MAC: `0100.0ccc.cccc`**.

👉 Switches don’t flood CDP frames; instead, they use them for their **CDP neighbor table**.


### 2.2 Information Shared by CDP

CDP messages advertise:

- **Device ID (Hostname)**

- **Capabilities (Device Type)**: Router (R), Switch (S), Host (H), Phone (P), Repeater (r), Bridge (B), etc.

- **Local Interface** (connection port on local device).
- **Neighbor’s Port ID** (interface on neighbor).
- **Holdtime** (time before entry expires).
- **Platform (Model)** → e.g., WS-C2960C.
- **Software Version (IOS version)**
- **VTP Domain, Native VLAN, Duplex Mode**
- **IP Address**


### 2.3 CDP Timers & Versions

- **Advertisement Timer**: Default = **6**0s**.

- **Holdtime**: Default = **180s** (entry removed if no update).

- **Versions**:
    - **CDPv2** (default on modern devices).
    - **CDPv1** (legacy support).

### 2.4 Configuring CDP

- Change advertisement timer:

```bash
cdp timer <seconds>
```

- Change holdtime:

```bash
cdp holdtime <seconds>
```

- Enable/disable CDPv2:

```bash
no cdp advertise-v2
```

- Disable globally:

```bash
no cdp run
```

- Disable per interface:

```bash
no cdp enable
```

### 2.5 Verifying CDP

- `show cdp neighbors` → Basic neighbor info (Device ID, Local Intf, Holdtme, Capability, Platform, Port ID).
- `show cdp neighbors detail` → Full info (IP, software version, VLAN, duplex).
- `show cdp entry <device>` → Detail for a specific neighbor.
- `show cdp` → Global status.
- `show cdp interface` → Status per interface.
- `show cdp traffic` → Statistics (sent/received messages, v1/v2 ads).


### 2.6 Security Considerations

CDP shares sensitive info (model, IOS version, IPs). Risks include:

- Attackers exploiting software vulnerabilities.

- Spoofing/disrupting link-layer events.


**Mitigation**:

- Disable CDP where not needed (`no cdp run` / `no cdp enable`).

- Use **802.1AE (MACsec)** for link protection.

- Consider **SEND (Secure Neighbor Discovery)** at L3.


### 2.7 Mapping a Network with CDP

By running **CDP show commands** across multiple devices, admins can **rebuild the entire network topology** including device roles, interfaces, and connections.


<h2 align="center">3. 🔹 Link Layer Discovery Protocol (LLDP)</h2>

### 3.1 Overview & Operation

- **IEEE 802.1AB standard**.

- Vendor-neutral, works in multi-vendor networks.

- Not enabled by default on Cisco devices.

- Multicast MAC: `0180.c200.000e`.

👉 Like CDP, switches process LLDP messages locally and update the **LLDP neighbor table**.


### 3.2 Information Shared by LLDP**

- **Hostname (System Name)**
- **Chassis ID**
- **Port ID + Description**
- **System Description (hardware/software)**
- **Holdtime**
- **System Capabilities (Router, Bridge, Station, etc.)**
- **Enabled Capabilities**
- **VLAN ID**


### 3.3 LLDP Timers

- Advertisement Timer: Default = **30s**

```bash
lldp timer <seconds>
```

- Holdtime: Default = 120s

```bash
lldp holdtime <seconds>
```

- Reinit Delay: Default = 2s (delay after enabling LLDP)

```bash
lldp reinit <seconds>
```

### 3.4 LLDP & Detecting Network Attachment (DNA)

- Defined in RFC 4957.
- Helps IP layers detect network configuration changes quickly.
- Uses link-layer event notifications (e.g., link up/down).
- System Capabilities TLV informs neighbors about device functions.
- Supports faster attachment (e.g., bypassing STP delay when safe).


### 3.5 Configuring LLDP

- **Enable globally**:

```bash
lldp run
```

- **Disable globally**:

```bash
no lldp run
```

- **Control per-interface (Tx/Rx)**:

```bash
lldp transmit
lldp receive
```


### 3.6 Verifying LLDP

- `show lldp` → Global status & timers.

- `show lldp interface` → Tx/Rx status.

- `show lldp traffic` → Statistics (sent/received, discarded, TLV errors).

- `show lldp neighbors` → Basic info (System Name, Port ID, Capabilities).

- `show lldp neighbors detail` → Detailed info (Chassis ID, software version, VLAN).

- `show lldp entry <device>` → Specific neighbor details.


### 3.7 CDP vs LLDP

| Feature       | **CDP**                     | **LLDP**                          |
| ------------- | --------------------------- | --------------------------------- |
| Standard      | Cisco Proprietary           | IEEE 802.1AB (open)               |
| Default       | Enabled                     | Disabled                          |
| Best Use Case | Cisco-only networks         | Multi-vendor networks             |
| Timers        | 60s (adv), 180s (hold)      | 30s (adv), 120s (hold), 2s reinit |
| Security      | Risky on untrusted ports    | Tx/Rx control gives flexibility   |
| Coexistence   | Can run both simultaneously | Can run both simultaneously       |


<h2 align="center">4. 🔹 Key Interview/Revision Points</h2>

- **CDP is ON by default**, LLDP is **OFF by default**.
- **MAC address difference**: CDP → `0100.0ccc.cccc`, LLDP → `0180.c200.000e`.
- **Timers**: CDP (60s/180s), LLDP (30s/120s/2s reinit).
- **Use CDP** for Cisco-only, LLDP for multi-vendor.
- **Both share device info** → disable on untrusted interfaces for security.
- **Verification commands** (`show cdp/lldp neighbors`, `detail`, `traffic`).


## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
