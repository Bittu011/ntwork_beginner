---
title: Layer 2 Interface
date: 2026-01-01 04:00:00 +0000
categories: [Palo Alto Firewall]
tags: [Layer 2, NGFW]
---


# Palo Alto Networks Study Notes  
## Layer 2 Interfaces and Integrated Switching


## 1. PAN-OS Training  
### Configuring Layer 2 Interfaces and Routing to Layer 3 via SVI

---

## 2. Overview

These notes explore how **Layer 2 (L2) interfaces** are implemented on a Palo Alto Networks firewall.  
This design allows the firewall to perform both **security** and **switching** functions within a single chassis — an ideal solution for **small branch offices** or environments without a dedicated switch.

The primary focus is:
- Grouping Layer 2 interfaces into **VLANs**
- Using a **Switched Virtual Interface (SVI)** to route traffic to **Layer 3 (L3)** for internet access

---

## 3. Beginner Notes (Simple Explanations)

### 🔹 What is a Layer 2 Interface?
In PAN-OS, a Layer 2 interface behaves like a port on a traditional switch.  
Instead of IP addresses, it forwards traffic using **MAC addresses** (physical device addresses).

---

### 🔹 Why Use Layer 2 on a Firewall?
If you have a small office without a separate switch:
- The firewall can **act as the switch**
- Still provides full **security inspection**
- Saves cost and simplifies design

---

### 🔹 VLANs (Virtual Local Area Networks)
VLANs create **logical boundaries**:
- Devices in the same VLAN can communicate directly
- Devices in different VLANs **cannot communicate** without Layer 3 routing

Think of VLANs as **separate rooms** inside the same building.

---

### 🔹 The “Bridge” (SVI)
To move traffic from **Layer 2 (local network)** to **Layer 3 (internet)**:
- You create a **VLAN Interface (SVI)**
- This virtual interface acts as a **bridge**
- It enables routing and security inspection

---

## 4. Intermediate Notes (How It Works & Configuration)

### 🔹 Layer 2 Operation
- The firewall maintains a **MAC address table**
- Traffic is forwarded based on hardware (MAC) addresses

---

### 🔹 Configuration Workflow

1. **Define Interface Type**  
   - Set physical interfaces (e.g., `ethernet1/1`) to **Layer 2**

2. **Assign a VLAN Object**  
   - Create a VLAN (e.g., `VLAN 10`)
   - Assign multiple L2 interfaces to it
   - Interfaces in the same VLAN can communicate at wire speed

3. **Security Zones**  
   - Create a **Layer 2 Security Zone**
   - L2 interfaces **cannot** be placed in L3 zones

---

### 🔹 The SVI (VLAN Interface)

- A **virtual Layer 3 interface** linked to a VLAN
- Assigned an **IP address**
- Acts as the **default gateway** for connected hosts
- Must be:
  - Assigned to a **Virtual Router**
  - Placed in a **Layer 3 Security Zone**

This is where Layer 2 traffic officially becomes **Layer 3 traffic**.

---

## 5. Advanced Notes (Design & Troubleshooting)

### 🔹 Spanning Tree Protocol (STP) Behavior

- The Palo Alto firewall **does not participate** in STP
- It does **not**:
  - Send BPDUs
  - Participate in root bridge elections
- It **transparently forwards** STP packets from other switches

✅ This prevents firewall ports from being placed in a **Blocking** state  
✅ Ensures uninterrupted traffic flow

---

### 🔹 Security Policy Requirements

❗ Security policies **cannot** be written directly from:
- **Layer 2 Zone → Layer 3 Zone**

#### Why?
- Once traffic reaches the **SVI**, it is considered **Layer 3**
- The zone applied to the SVI becomes the traffic’s zone

#### Correct Policy Design

-   Inside-L3-Zone → Outside-L3-Zone


---

### 🔹 NAT Implementation

- Source NAT (PAT) is required for internet access
- NAT is configured on the **egress L3 interface**
- If the internet interface uses **DHCP**:
  - Use **Interface Address** as the translated source

---

## 6. Key Terms

| Term | Description |
|----|----|
| **Layer 2 Interface** | Operates at the Data Link layer using MAC addresses |
| **VLAN** | Logical broadcast domain grouping L2 interfaces |
| **SVI (VLAN Interface)** | Provides Layer 3 routing for a Layer 2 VLAN |
| **MAC Table** | Maps MAC addresses to physical ports |
| **STP Transparency** | Firewall forwards STP without participating |

---

## 7. Quick Revision Summary

- **Consolidation**: Firewalls can replace switches in small environments
- **Connectivity**: L2 ports in the same VLAN and L2 zone communicate directly
- **Routing Path**: L2 Interface → VLAN → SVI → Virtual Router → L3 Egress Interface

- **Gateway**: SVI IP = Default Gateway for L2 hosts
- **Policies**: Security rules must be written between **L3 zones**
- **STP**: Firewall remains transparent to avoid port blocking

---

📘 *End of Notes*



# 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)

