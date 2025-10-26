---
title:  IPv4 Addressing & Subnetting
date: 2025-10-20 04:00:00 +0000
categories: [CCNA]
tags: [ccna, ntworkengineer, ccnp enarsi, ccnp, ipv4, subnetting]
---



<h1 align="center">🧠 COMPREHENSIVE NOTES ON IPv4 ADDRESSING & SUBNETTING</h1>



## 🧩 I. FUNDAMENTALS OF IP ADDRESSING

### 🔹 A. Why IP Addresses Are Needed
- The Internet Protocol (IP) assigns unique identifiers to devices so they can exchange data.  
- In a simple two-device (point-to-point) link, identifiers aren’t needed since data flows directly.  
- In any network with three or more devices, **unique addresses** are essential for correct delivery.

### 🔹 B. Address Function Across OSI Layers

| Layer | Type of Address | Description |
|:------|:----------------|:-------------|
| **Layer 2 (Data Link)** | MAC Address | Unique per device; doesn’t indicate location (like a name). |
| **Layer 3 (Network)** | IP Address (IPv4/IPv6) | Indicates both *who* and *where* a device is located. |

When data moves across a network, IP encapsulates it in a **packet**, tagging both:
- **Source IP:** Origin of the packet  
- **Destination IP:** Target device’s address



## 🌐 II. IPv4 ADDRESS STRUCTURE

- **Total Length:** 32 bits  
- **Human Representation:** Four octets in dotted-decimal form (e.g., `192.168.1.10`)  
- **Binary Division:** 4 octets × 8 bits = 32 bits  

Each IPv4 address has:
1. **Network Portion:** Identifies the network segment (shared by all devices in that subnet).  
2. **Host Portion:** Uniquely identifies the device within that network.


## 🧮 III. ROLE OF THE SUBNET MASK

### 🔹 A. Function of a Subnet Mask
- **Definition:** A 32-bit number that determines which part of the IP address represents the *network* vs. the *host*.
- **Display:** Dotted-decimal form (e.g., `255.255.255.0`).
- **Rules of Comparison:**
  - `255` → Corresponding bits are **network** bits.
  - `0` → Corresponding bits are **host** bits.

### 🔹 Routing Logic
- If the destination is **within the same subnet**, packets are sent directly.  
- If **outside**, they’re forwarded to the **default gateway** (router).

### 🔹 B. IPv6 Comparison

| Feature | IPv4 | IPv6 |
|:---------|:------|:------|
| **Address Length** | 32 bits | 128 bits |
| **Format** | Dotted-decimal | Hexadecimal with colons |
| **Subnet Notation** | Mask or `/` slash (e.g., `/24`) | Always uses `/` notation (e.g., `/64`) |
| **Parts** | Network + Host | Prefix + Interface ID |


## 🌍 IV. TYPES OF IPv4 ADDRESSES

### 🔹 A. Public vs. Private

| Type | Purpose | Address Range |
|:------|:----------|:----------------|
| **Public** | Globally unique, routable on the Internet. | Any address *not* in private range (1.0.0.0–9.x.x.x, 11.x.x.x–126.x.x.x, 128.x.x.x–223.x.x.x). |
| **Private** | Used only within local networks. Not Internet-routable. | `10.0.0.0 – 10.255.255.255` <br> `172.16.0.0 – 172.31.255.255` <br> `192.168.0.0 – 192.168.255.255` |

### 🔹 B. Reserved IPv4 Addresses

| Type | Rule | Example |
|:------|:------|:---------|
| **Network Address** | All host bits = `0` | `20.1.17.0/24` |
| **Broadcast Address** | All host bits = `1` | `20.1.17.255` |
| **Limited Broadcast** | Reaches all local devices; not routed beyond local network. | `255.255.255.255` |

> ⚠️ Note: IPv6 does **not** use broadcast addresses.



## 🖥️ V. ADDRESS ASSIGNMENT METHODS

| Method | Description |
|:--------|:-------------|
| **DHCP (Dynamic)** | Device requests IP, subnet mask, gateway, and DNS from DHCP server. |
| **Static** | Manually assigned and unchanging; common for servers and routers. |
| **APIPA (Automatic Private IP Addressing)** | Self-assigned IP (`169.254.x.x`) when DHCP fails — indicates a connectivity issue. |



## 🧱 VI. SUBNETTING

### 🔹 A. Why Subnetting Is Used
- To **segment large networks** for better **security**, **management**, and **traffic control**.  
- Example: Separating HR, Finance, and Engineering into their own subnets.

### 🔹 B. How Subnetting Works
- Subnetting involves **extending the subnet mask** by converting host bits into network bits.  
- This allows multiple smaller subnets under one larger parent network.

### 🔹 C. Subnet Calculation (The “Finger Method”)
1. Determine the required number of subnets.  
2. Use powers of two to calculate how many bits to borrow:
   - 1 bit → 2 subnets  
   - 2 bits → 4 subnets  
   - 3 bits → 8 subnets  
   - 4 bits → 16 subnets  
   - 5 bits → 32 subnets  
3. Add the borrowed bits to the existing mask to find the **new subnet mask**.

### 🔹 D. Special Case: /31 Subnet Mask
- Used for **point-to-point** links (two devices only).  
- Leaves just **1 host bit**, enabling exactly **2 usable addresses**.  
- Routers treat both addresses as valid (breaking the typical “no all-zeros/all-ones” rule).



## ⚙️ VII. RELATED NETWORKING CONCEPTS

| Concept | Description |
|:----------|:-------------|
| **DNS (Domain Name System)** | Translates human-readable names into IP addresses. Example: `8.8.8.8` (Google DNS). |
| **Wireshark** | Packet capture and analysis tool showing real-time source and destination IPs. |
| **Wildcard Mask** | Used mainly in ACLs (Access Control Lists). Reverse of a subnet mask: <br> `0` = must match, `1` = ignore bit. Example: Subnet Mask `255.255.255.0` → Wildcard Mask `0.0.0.255`. |



## 💡 QUICK RECAP: IPv4 Essentials for Interview & Revision 


✅ IPv4 = 32-bit addressing system using dotted-decimal format.  
✅ Subnet Mask determines Network vs. Host bits.  
✅ Private IPs are for internal use only.  
✅ Broadcast & Network addresses are reserved.  
✅ Subnetting enhances efficiency, control, and security.  
✅ /31 mask is optimized for router-to-router links.  
✅ DHCP automates IP assignment; APIPA indicates DHCP failure.  
✅ DNS resolves names; Wireshark inspects packets.  
✅ Wildcard Masks are used for security filtering.

---

📘 **Created for:**
- Learners preparing for **networking certifications (CCNA, CompTIA, etc.)**  
- Interviewers seeking **precise technical evaluation points**  
- Engineers wanting **quick refreshers** on IPv4 and subnetting fundamentals  




## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
