---
title: Network Device
date: 2025-07-15 07:00:00 +0000
categories: [Networking_Topics]
tags: [network_device, ccna, cisco]
---

## Network Devices

### 1. What is a Network?

- A **computer network** is a telecommunications network that allows **nodes to share resources**.
- **LAN (Local Area Network):** Group of interconnected devices in a limited area (office, campus).
- **WAN (Wide Area Network):** Extends over large geographical areas (cities, countries).
- **Node:** Any device connected to a network (PC, iPhone, router, etc.).
- **Resource:** Anything that can be accessed/used over the network.

---

### 2. Network Devices

### **Client & Server**

- **Client:** A device that accesses a service provided by a server.
- **Server:** A device that provides services for clients.
- Client & server nodes are often called **endpoints / end hosts**.

---

### **Hub**

- **Definition:** Simple device to connect multiple devices in a LAN.
- **Function:** Broadcasts incoming data to all ports.
- **Limitation:** Causes unnecessary traffic → rarely used in modern networks.
- **Layer:** OSI Layer 1 (Physical).

---

### **Switch**

- **Definition:** Switches provide connectivity between devices in a LAN.
- Typically have **24–48 ports** for end hosts (PCs, printers, servers, cameras).
- **Function:** Forwards data only to intended recipient using **MAC addresses**.
- **Layer:** OSI Layer 2 (Data Link).
- **Note:** A port is a physical connector on a device (used interchangeably with interface).

{ ⇒ Devices connected to a switch are able to communicate with each other via the switch. Note that they do not typically communicate with the switch itself—the switch only serves as infrastructure over which communication can occur.

The role of a switch is to connect devices within a LAN. For example, all of the PCs, security cameras, printers, servers, and other devices in an office are probably connected to one or more switches. For this reason, it’s common for switches to have many ports for end hosts to connect to—usually from 24 to 48 per switch.

NOTE A port is a physical connector on a device. Devices are physically connected by connecting one end of a cable to each of two devices. A port serves as the interface between one device and the other devices in the network. For that reason, the terms port and interface are often used interchangeably. }

---

### **Router**

- **Definition:** Routers provide connectivity between LANs and external networks (e.g., Internet).
- **Function:** Forwards packets between networks based on **IP addresses**.
- **Layer:** OSI Layer 3 (Network).

**Wireless Routers (SOHO):**

> A wireless router (Wi-Fi router/home router) is not just a router; it’s a multifunctional device combining router + switch + wireless access point + firewall.
> 
> 
> Common in **small office/home office (SOHO)**, but enterprises use separate dedicated devices.
> 

---

### **Firewall**

- **Definition:** Firewalls secure the network by inspecting traffic entering/exiting and allowing/denying based on rules.

**Types:**

- **Host-based Firewall** → Software firewall (e.g., Windows Defender). Works on a single device.
- **Network Firewall** → Dedicated hardware. Protects entire network by filtering all inbound/outbound traffic.

---

### **Wireless Access Point (AP)**

- **Role:** Connect wireless devices to wired network.
- **Function:** Provides **WLAN (Wi-Fi)** connectivity.
- **Layer:** OSI Layer 2.

---

### **Modem**

- **Definition:** Converts digital data ↔ analog signals for transmission over phone/cable lines.
- **Use:** DSL, cable, or dial-up internet.

---

### **Repeater**

- **Definition:** A repeater regenerates/amplifies signals to extend travel distance.
- **Function:** Prevents signal degradation across long distances.
- **Use Case:** Extend Ethernet or Wi-Fi.
- **Layer:** OSI Layer 1.

---

### **Bridge**

- **Definition:** A bridge connects and filters traffic between two network segments, making them function as one.
- **Function:** Creates separate **collision domains**, reduces unnecessary traffic.
- **Layer:** OSI Layer 2.
- **Use:** Break down large networks into smaller, manageable sections.

---

### **Gateway**

- **Definition:** A gateway acts as an entry/exit point between different networks, often operating at multiple OSI layers.
- **Function:** Protocol/format conversion (e.g., TCP/IP ↔ AppleTalk).
- **Use:** LAN-to-Internet, or between different architectures.
- **Note:** Routers often integrate gateway functions.

---

### **WLC (Wireless LAN Controller)**

- **Definition:** Used to manage multiple access points.
- **Function:** Centralized control, configuration, and monitoring of Wi-Fi networks.

---

### 3. Network Topologies

- **Definition:** Topology = physical or logical arrangement of devices and connections.

**Types:**

- **Bus:** Single line, one failure = full outage.
- **Star:** All devices connect to central hub/switch; hub failure = outage.
- **Ring:** Devices in circular chain; data passes device-to-device.
- **Mesh:** Devices fully interconnected; very reliable but costly.

---

### 4. Network Interface Card (NIC)

- **Definition:** Hardware inside end devices enabling network connectivity.
- **Types:** Wired (Ethernet), Wireless (Wi-Fi).
- **Each NIC has a unique MAC address.**
- May be integrated into motherboard or add-on card.

---

### 5. Message Delivery Types

- **Unicast:** One-to-one transmission.
- **Broadcast:** One-to-all in a broadcast domain (routers block broadcasts by default).
- **Multicast:** One-to-many group communication; efficient bandwidth use.
- **Protocols:** IGMP (IPv4), MLD (IPv6).

---

### 6. Quick Command Reference (Windows CMD)

| Command | Purpose |
| --- | --- |
| `ping` | Check reachability (ICMP). |
| `ipconfig` | Show IP address. |
| `ipconfig /all` | Show IP + MAC + full details. |
| `getmac` | Show MAC address only. |
| `netstat` | Show active connections/sessions. |
| `nslookup` | Query DNS records for a website. |
| `arp -a` | Show ARP table (IP-to-MAC mapping). |
| `arp -d` | Clear ARP table (Admin mode). |

### **Summary Table: Key Network Devices**
| Order | Topic                           | Brief Description                               | Key Details / Typical Use                         |
|------:|---------------------------------|--------------------------------------------------|---------------------------------------------------|
| 1     | Network Devices                 | Basic hardware in networks (NIC, switch, router, hub, AP) | Connect, forward, and manage data traffic         |
| 2     | Network Topology                | Physical/logical arrangement of network connectivity | Star, bus, ring, mesh; affects efficiency         |
| 3     | Network Interface Card (NIC)    | Hardware enabling device network access          | Unique MAC, installed on end devices              |
| 4     | Repeater                        | Signal booster to extend network length          | Ethernet extension, Layer 1 device                |
| 5     | Bridge                          | Connects/filter traffic between network segments | Splits collision domains, Layer 2 device          |
| 6     | Gateway                         | Connects different networks/protocols            | LAN-to-Internet, protocol conversion              |
| 7     | Message Delivery Types          | Data delivery forms: unicast, broadcast, multicast | 1-to-1, 1-to-all, 1-to-group transmission        |


## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
