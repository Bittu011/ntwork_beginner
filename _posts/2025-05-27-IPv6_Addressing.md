---
title: IPv6 Addressing
date: 2025-05-27 04:00:00 +0000
categories: [CCNA]
tags: [ipv6, ccna, ccnp, cisco, network_topics]
---


<h1 align="center">🌐 IPv6 Deep Dive: Addressing the Future of the Internet</h1>

IPv6 (Internet Protocol version 6) is the modern backbone of the internet’s addressing system. This comprehensive guide explains every IPv6 concept—perfect for learners, interview preparation, and network engineers revising IPv6 fundamentals.



<h2 align="center">1️⃣ Why We Need IPv6</h2>

The evolution to IPv6 arose from the limitations of IPv4.


**⚙️ IPv4 Limitations**

-   **IPv4 Address Exhaustion**:
    -   IPv4 uses a 32-bit address, supporting roughly 4.3 billion unique addresses (2^32). With the exponential growth of internet-connected devices,  this pool is now insufficient.

-   **Reserved Ranges:**
    -   Not all IPv4 addresses are usable—Class D is for multicast and Class E for experimental use.

-   **Need for a Scalable Solution:**
    -   IPv6 offers 128-bit addressing, allowing 2^128 (~340 undecillion) unique addresses—effectively infinite for modern and future networking needs.



<h2 align="center">2️⃣ IPv6 Address Structure and Notation</h2>


IPv6 addresses are written in hexadecimal and structured differently from IPv4.


<h3 align="center">🧩 Address Format</h3>

-   **Structure**: 8 groups (hextets) of 16 bits, separated by colons `:`.

-   **Example**: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`

-   **Prefix Length**: Always denoted with slash notation (e.g., `/64`).

-   **Network & Host Portions**:
    -   **First 64 bits** → Network prefix
    -   **Last 64 bits** → Interface Identifier (host portion)

>  💡 Example: Address `2001:db8:1:2:a:b:c:d/64` → Network prefix is `2001:db8:1:2::/64`


<h3 align="center">✂️ Abbreviation Rules</h3>


1.  Remove Leading Zeros
    -   `0db8 → db8`, `0000 → 0`

2.  Compress Consecutive Zeros
    -   Replace a single sequence of consecutive zero hextets with `::`
    -   (can be used only once per address)

>   Example: 2001:0db8:0000:0000:0000:0000:0000:1 → 2001:db8::1


<h3 align="center">🧭 RFC 5952 Guidelines</h3>


-   Remove all leading zeros.
-   Use lowercase a–f.
-   Compress the longest sequence of zeros.


<h2 align="center">3️⃣ IPv6 Header: Simplicity and Efficiency</h2>


IPv6 introduces a fixed-length header (40 bytes), optimizing packet processing.


<h3 align="center">🧱 Key Header Fields</h3>


| Field                    | Description                                                |
| ------------------------ | ---------------------------------------------------------- |
| **Version**              | Always `6`                                                 |
| **Traffic Class**        | QoS (similar to IPv4 Type of Service)                      |
| **Payload Length**       | Size of encapsulated data                                  |
| **Next Header**          | Identifies upper-layer protocol (TCP=6, UDP=17, ICMPv6=58) |
| **Hop Limit**            | Replaces TTL; decrements at each router                    |
| **Source & Destination** | 128-bit IPv6 addresses                                     |


<h2 align="center">4️⃣ Understanding Hexadecimal Conversion</h2>


IPv6 addresses use hexadecimal; understanding conversions is crucial.


| Decimal | Binary | Hex |
| ------- | ------ | --- |
| 10      | 1010   | A   |
| 15      | 1111   | F   |


<h3 align="center">🔄 Conversions</h3>


**Binary → Hexadecimal**:

1.  Split into 4-bit groups

2.  Convert each to decimal

3.  Then to hexadecimal


**Hexadecimal → Binary**:

1.  Split each hex digit
2.  Convert to 4-bit binary equivalent


<h2 align="center">5️⃣ IPv6 Address Types</h2>


IPv6 defines five primary address types—each with distinct purposes.


| Type                     | Purpose                                    | Range / Example  | IPv4 Equivalent     |
| ------------------------ | ------------------------------------------ | ---------------- | ------------------- |
| **Global Unicast (GUA)** | Internet-routable unique addresses         | `2000::/3`       | Public IPv4         |
| **Unique Local (ULA)**   | Private internal networks                  | `fd00::/8`       | Private IPv4        |
| **Link-Local (LLA)**     | Communication on local link only           | `fe80::/64`      | APIPA (169.254.x.x) |
| **Multicast**            | One-to-many communication                  | `ff00::/8`       | IPv4 Multicast      |
| **Anycast**              | One-to-one-of-many (closest host responds) | No special range | None (new in IPv6)  |


<h3 align="center">⚡ Reserved Special Addresses</h3>


-   **Unspecified**: `::` → Used when a device doesn’t know its address or as default route (`::/0`)
-   **Loopback**: `::1` → Used for local testing (like `127.0.0.1` in IPv4)


<h2 align="center">6️⃣ IPv6 Configuration and EUI-64</h2>


IPv6 configuration syntax resembles IPv4, replacing `ip` with `ipv6`.


<h3 align="center">🧑‍💻 Basic Cisco Commands</h3>


```bash
Router(config)# ipv6 unicast-routing        # Enable IPv6 routing
Router(config-if)# ipv6 address 2001:db8::1/64
Router(config-if)# ipv6 address 2001:db8::2/64 eui-64
```

<h3 align="center">🧮 Modified EUI-64 Addressing</h3>


Generates the Interface ID automatically from the device’s MAC address:

1.  Split MAC into halves
2.  Insert `fffe` in the middle
3.  Flip the 7th bit (U/L bit)

> Example: MAC: `00:1A:2B:3C:4D:5E` → Interface ID: `021a:2bff:fe3c:4d5e`


<h2 align="center">7️⃣ IPv6 Routing and Neighbor Discovery Protocol (NDP)</h2>


IPv6 replaces ARP with NDP (Neighbor Discovery Protocol), part of ICMPv6.


<h3 align="center">🔍 NDP Functions</h3>


| Function                        | Description                | ICMPv6 Type |
| ------------------------------- | -------------------------- | ----------- |
| **Neighbor Solicitation (NS)**  | ARP Request equivalent     | 135         |
| **Neighbor Advertisement (NA)** | ARP Reply equivalent       | 136         |
| **Router Solicitation (RS)**    | Hosts request router info  | 133         |
| **Router Advertisement (RA)**   | Routers advertise prefixes | 134         |


<h3 align="center">📡 Solicited-Node Multicast</h3>


-   Used for ARP-like queries
-   Format: `ff02::1:ffXX:XXXX` (last 24 bits of unicast address)


<h3 align="center">🧠 SLAAC (Stateless Address Autoconfiguration)</h3>


Allows automatic self-addressing:

1.  Host receives prefix from RA
2.  Combines prefix with EUI-64 or random ID


<h3 align="center">🧾 Duplicate Address Detection (DAD)</h3>


Ensures the address is unique before use by sending NS to its own solicited-node multicast address.



<h2 align="center">8️⃣ IPv6 Routing Table Overview</h2>


IPv6 routers follow the Longest Prefix Match (LPM) rule.


<h3 align="center">📘 Route Types</h3>


| Code  | Type      | Description                  |
| ----- | --------- | ---------------------------- |
| **C** | Connected | Network directly connected   |
| **L** | Local     | Host-specific (/128)         |
| **S** | Static    | Manually configured          |
| **D** | Dynamic   | Learned via routing protocol |



<h2 align="center">9️⃣ IPv6 Static Routing</h2>


<h3 align="center">⚙️ Command Syntax</h3>

```bash
Router(config)# ipv6 route <destination-prefix> <next-hop / exit-interface>
```

| Type                      | Description               | Example                                      |
| ------------------------- | ------------------------- | -------------------------------------------- |
| **Recursive**             | Next-hop only             | `ipv6 route 2001:db8:2::/64 2001:db8:1::2`   |
| **Directly Connected**    | Exit interface only       | `ipv6 route 2001:db8:2::/64 g0/0`            |
| **Fully Specified**       | Both interface & next-hop | `ipv6 route 2001:db8:2::/64 g0/0 fe80::2`    |
| **Default Route**         | Catch-all route           | `ipv6 route ::/0 fe80::2 g0/0`               |
| **Floating Static Route** | Backup route (higher AD)  | `ipv6 route 2001:db8:2::/64 2001:db8:1::2 2` |


>   🧭 Note: When using Link-Local Addresses (LLA) as next hops, the exit interface must be specified.


<h2 align="center">🧩 Quick IPv6 Interview Revision Sheet</h2>


| Concept             | Quick Definition                                  |
| ------------------- | ------------------------------------------------- |
| IPv6 Length         | 128 bits                                          |
| Header Size         | Fixed 40 bytes                                    |
| Broadcast           | **No broadcast in IPv6** (uses multicast instead) |
| Address Auto-Config | **SLAAC**                                         |
| Replacement for ARP | **NDP (ICMPv6)**                                  |
| Loopback            | `::1`                                             |
| Default Route       | `::/0`                                            |
| Private Range       | `fd00::/8`                                        |
| Link-Local Range    | `fe80::/64`                                       |
| Global Range        | `2000::/3`                                        |


<h2 align="center">📚 Summary</h2>


IPv6 is not just an upgrade—it’s the foundation of the modern Internet, ensuring scalability, efficiency, and robust address management. From addressing structure and compression to routing and NDP, understanding IPv6 is a must-have skill for any network engineer or SOC analyst.




## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
