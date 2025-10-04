---
title: DHCP Packet Level
date: 2025-08-12 04:00:00 +0000
categories: [Networking_Topics]
tags: [dhcp, packet_level, ccna, ccnp, networking]
---

# DHCP Deep Dive — From Basics to Packet-Level (For Network Engineers)

> This guide is for **network engineers** who want to deeply understand DHCP — from its core function to full packet-level inspection. It's designed for readers who may be new to DHCP or those who want a deeper understanding of how it works behind the scenes in real-world networks.

---

## Table of Contents

1. What is DHCP and why it's needed  
2. Prerequisites: MAC, IP, UDP, broadcast  
3. Evolution: BOOTP → DHCP → PXE  
4. DHCP Roles and Packet Flow Overview  
5. DHCP Packet Structure (ASCII Format)  
6. DHCP Options: Format, Usage, Common Options  
7. The DORA Process (Discover → Offer → Request → ACK)  
8. Other DHCP Message Types Explained  
9. DHCP Relay (giaddr, hops, Option 82)  
10. PXE Boot Overview (DHCP Options 66 & 67)  
11. Packet Capture and Analysis Tools  
12. Server Configuration Examples  
13. Troubleshooting Checklist  
14. Appendix: Glossary & Resources

---

## 1. What is DHCP?

**DHCP (Dynamic Host Configuration Protocol)** allows devices to receive IP address configuration automatically on a network.

Without DHCP, every device would need manual configuration — error-prone, inefficient, and impractical at scale. DHCP automates this by dynamically leasing IP addresses and providing:

- IP address
- Subnet mask
- Default gateway
- DNS servers
- And other options (via extensible DHCP options)

---

## 2. Prerequisites (Quick Refresher)

- **MAC Address**: Hardware address (Layer 2). DHCP clients identify themselves using their MACs.
- **IP Address**: Logical address (Layer 3). Clients may not have one yet when they request DHCP.
- **Broadcast**: DHCP clients often send messages to `255.255.255.255` to reach servers on the local subnet.
- **UDP Ports**: DHCP uses:
  - UDP port **67** (server)
  - UDP port **68** (client)

---

## 3. BOOTP → DHCP → PXE

### BOOTP (1985)
- Static mapping between MAC and IP
- Used mainly for bootstrapping diskless clients
- Predecessor to DHCP

### DHCP (1993, RFC 2131)
- Dynamic address leasing
- Based on BOOTP format (reuses same structure)
- Extends BOOTP with lease management and options

### PXE (Preboot eXecution Environment)
- Uses DHCP to locate boot servers/files (via options 66 & 67)
- Allows devices to boot over the network

---

## 4. DHCP Roles & Packet Flow

### Devices involved:

- **Client**: Requests configuration (e.g., PC, phone, printer)
- **Server**: Allocates IP and options
- **Relay Agent**: Forwards DHCP messages between subnets

### UDP Ports:

- Client: **68**
- Server: **67**

### Common DHCPv4 Message Types:

| Message Type | Code | Description                      |
|--------------|------|----------------------------------|
| DISCOVER     | 1    | Client looking for IP offer      |
| OFFER        | 2    | Server offers IP + config        |
| REQUEST      | 3    | Client requests specific offer   |
| ACK          | 5    | Server confirms the assignment   |

### DORA Sequence:

1. **DHCPDISCOVER** (Client → Broadcast)
2. **DHCPOFFER** (Server → Client)
3. **DHCPREQUEST** (Client → Server)
4. **DHCPACK** (Server → Client)

---

## 5. DHCP Packet Structure (ASCII Layout)

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|     op      |    htype     |    hlen      |     hops         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                          xid (Transaction ID)                |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           secs              |           flags               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      ciaddr (Client IP address)              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      yiaddr ("Your" IP address)              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      siaddr (Server IP address)              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      giaddr (Gateway IP address)             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                    chaddr (Client MAC address)               |
|                                                               |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                    sname (Optional Server Hostname)          |
|                         [64 bytes]                           |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                    file (Boot File Name)                     |
|                         [128 bytes]                          |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                    options (Variable Length)                 |
|             Starts with 4-byte magic cookie (DHCP)           |
|              Then a sequence of TLV DHCP options             |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

## 6. DHCP Options Overview

### Format (TLV):

> | Code (1 byte) | Length (1 byte) | Value (Length bytes) |


### Special:
- `0`: Pad
- `255`: End of options

### Common Options:

| Code | Option                     | Description                  |
|------|----------------------------|------------------------------|
| 1    | Subnet Mask                | Usually 255.255.255.0        |
| 3    | Router (Gateway)           | Default gateway              |
| 6    | DNS Servers                | Up to 2 or more addresses    |
| 12   | Hostname                   | Client hostname              |
| 50   | Requested IP Address       | Used in REQUEST              |
| 51   | Lease Time                 | In seconds                   |
| 53   | DHCP Message Type          | 1=DISCOVER, 2=OFFER, etc.    |
| 54   | DHCP Server Identifier     | Server's IP                  |
| 55   | Parameter Request List     | List of options client wants |
| 66   | TFTP Server Name (PXE)     | PXE-related                  |
| 67   | Bootfile Name (PXE)        | PXE boot filename            |

---

## 7. DORA: Step-by-Step Packet Flow

1. **DHCPDISCOVER**:
   - From client (MAC only, no IP)
   - Broadcast to 255.255.255.255
   - Options: Message type = 1, Parameter Request List

2. **DHCPOFFER**:
   - From server
   - Contains `yiaddr` (offered IP), subnet mask, router, DNS
   - Includes server ID (option 54)

3. **DHCPREQUEST**:
   - Client chooses a server (from OFFERs)
   - Broadcasts REQUEST with:
     - Requested IP (option 50)
     - Server ID (option 54)

4. **DHCPACK**:
   - Server confirms lease
   - Client applies configuration

---

## 8. Other Message Types (Brief)

| Type        | Description |
|-------------|-------------|
| DHCPDECLINE | Client tells server an IP is already in use (e.g. via ARP check) |
| DHCPRELEASE | Client releases the IP |
| DHCPINFORM  | Client has static IP but wants config options (like DNS) |

---

## 9. DHCP Relay Agents & Option 82

When the client and server are on different subnets, DHCP **relay agents** (usually routers or switches) forward messages to the server.

### Relay Behavior:

- Sets `giaddr` = relay's interface IP
- Forwards client’s broadcast as **unicast** to DHCP server
- Receives reply from server and sends it back to client

### Option 82 (Relay Agent Info):

- Optional suboptions like Circuit ID or Remote ID
- Switches can insert Option 82 for identification
- Server can use it to apply pool/policy per port/VLAN

---

## 10. PXE Boot & DHCP

PXE clients use DHCP to get boot info (along with IP).

Required options:

- **Option 66**: TFTP server (e.g., `192.168.1.10`)
- **Option 67**: Boot file (e.g., `pxelinux.0`, `bootx64.efi`)

Some setups use two DHCP servers:

- **Standard DHCP server**: provides IP
- **Proxy DHCP (PXE)**: provides only PXE options (no IP)

---

## 11. Packet Capture & Analysis

### Tools:

- `tcpdump`
- `tshark`
- Wireshark

### Commands:

```bash
# Basic DHCP capture
sudo tcpdump -i eth0 port 67 or port 68 -n

# Save to file
sudo tcpdump -i eth0 -w dhcp.pcap port 67 or 68

# Read capture with tshark
tshark -r dhcp.pcap -Y "bootp"
```

## Wireshark Filters:

| **Purpose**     | **Filter**                     |
|------------------|-------------------------------|
| All DHCP         | `bootp`                       |
| Discover only    | `bootp.option.type == 1`      |
| Offer only       | `bootp.option.type == 2`      |

---

## 12. DHCP Server Config Examples

### Cisco Router:

```bash
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool OFFICE
  network 192.168.1.0 255.255.255.0
  default-router 192.168.1.1
  dns-server 8.8.8.8
  lease 8
```

### Linux (isc-dhcp-server):

```bash
subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option domain-name-servers 8.8.8.8;
  default-lease-time 600;
  max-lease-time 7200;
}
```

## 13. Troubleshooting DHCP

| **Problem**              | **Check**                                      |
|--------------------------|------------------------------------------------|
| Client not getting IP    | Is server reachable?                           |
| Only works in some VLANs | Is relay (`giaddr`) configured?                |
| Server not responding    | Filtered by firewall?                          |
| Packet drops seen        | Use `tcpdump` to confirm                       |
| Wrong IP assigned        | Pool configuration correct?                    |
| Slow PXE boot            | TFTP reachable? Correct options?               |

---

## 14. Glossary

- **DHCP**: Dynamic Host Configuration Protocol  
- **BOOTP**: Bootstrap Protocol (pre-DHCP)  
- **PXE**: Preboot Execution Environment  
- **giaddr**: Gateway IP address (for relays)  
- **xid**: Transaction ID  
- **chaddr**: Client hardware address  

## Further Reading
- [RFC 2131 - DHCP for IPv4](https://tools.ietf.org/html/rfc2131)
- [Wireshark BootP/DHCP Display Filters](https://wiki.wireshark.org/Bootp)
- [DHCP Options List - IANA](https://www.iana.org/assignments/dhcp-parameters/dhcp-parameters.xhtml)



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
