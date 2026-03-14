---
title: DHCP Snooping
date: 2026-02-20 04:00:00 +0000
categories: [CCNA]
tags: [DHCP Snooping, CCNA]
---

# DHCP Snooping – The Network Guardian 🛡️  
*A Creative Deep Dive for Engineers, Students, and Curious Minds*

Imagine your network as a **busy city**.

Devices are citizens.  
IP addresses are home addresses.  
And the **DHCP server** is the **city registrar** assigning those addresses so everyone knows where to live.

But what if a **fake registrar appears** and starts handing out wrong addresses?  
Or what if criminals flood the office with fake requests so **real citizens can't get homes**?

This is exactly where **DHCP Snooping** becomes the **guardian of the network**.

In this blog, we will break down DHCP Snooping in a **story-driven and easy-to-revise way** so you can understand the concept deeply and quickly recall it during troubleshooting, design, or certification exams.

---

# 1. The Battlefield: DHCP-Based Attacks ⚔️

Before understanding the defense, we must understand the **enemy**.

DHCP is a **trust-based protocol**, which makes it vulnerable to attacks.

Two common attacks exploit this weakness.

---

## 1.1 DHCP Poisoning (Man-in-the-Middle)

In this attack, a **rogue DHCP server** appears inside the network.

Here is what happens:

1. A client sends a **DHCP DISCOVER**
2. Both legitimate and rogue servers respond
3. The rogue server responds **faster**
4. The client accepts the rogue response

The attacker then provides:

- Valid IP address
- Malicious **default gateway**

Now the attacker sits **between the client and the network**, allowing them to:

- Monitor traffic
- Modify packets
- Launch further attacks

This is a classic **Man-in-the-Middle (MITM)** scenario.

---

## 1.2 DHCP Exhaustion (Starvation)

This attack targets the **availability of IP addresses**.

The attacker floods the network with thousands of : DHCP DISCOVER


Each request uses a **spoofed MAC address**.

The DHCP server thinks these are real clients and starts allocating IP addresses.

Eventually : IP Pool → Fully Consumed


Result:

Legitimate users cannot obtain IP addresses and **lose network connectivity**.

---

# 2. The Core Logic: Trusted vs Untrusted Ports 🔐

The foundation of DHCP Snooping is very simple but extremely powerful.

**Every switch port is classified as either:**

```bash
Trusted
or
Untrusted
```


---

## 2.1 Untrusted Ports (Default)

These are ports connected to:

- PCs
- Laptops
- Printers
- End devices

The switch **does NOT trust these ports**.

Expected traffic : DHCP Client Messages Only


Examples:

- DISCOVER
- REQUEST

---

## 2.2 Trusted Ports

These ports connect to:

- Legitimate DHCP servers
- Uplink switches
- Infrastructure devices

Trusted ports allow **both client and server DHCP messages**.

---

# 3. Filtering Rules on Untrusted Ports 🚧

Once DHCP Snooping is enabled, the switch begins **inspecting DHCP packets**.

Different rules apply depending on the message type.

---

## 3.1 Server Messages Are Blocked

If a server-type message appears on an untrusted port, the switch **drops it immediately**.

Blocked messages:

```bash
DHCPOFFER
DHCPACK
DHCPNAK
```


Purpose : Prevent rogue DHCP servers


---

## 3.2 Client Message Validation

Client messages such as:

```bash
DISCOVER
REQUEST
```


are allowed **only if one condition is true** : Ethernet Source MAC == DHCP chaddr field


This ensures that attackers **cannot spoof DHCP requests easily**.

---

## 3.3 Release and Decline Validation

DHCP Snooping also verifies:

```bash
DHCP RELEASE
DHCP DECLINE
```


These messages must match an existing entry in the **DHCP Snooping Binding Table**.

If they don't match : Packet → Dropped



This prevents malicious devices from **releasing someone else's IP address**.

---

# 4. The Brain: DHCP Snooping Binding Table 🧠

As the switch observes valid DHCP conversations, it builds a **dynamic database** called the **Binding Table**.

This table stores:

| Field | Description |
|-----|-----|
| Client MAC | Device hardware address |
| IP Address | Assigned IP |
| VLAN ID | VLAN where the client exists |
| Interface | Switch port |
| Lease Time | Duration of DHCP lease |

Example conceptual entry:

```bash
MAC: 00:1A:2B:3C:4D:5E
IP: 192.168.10.25
VLAN: 10
Interface: Gi0/12
Lease: 86400 seconds
```


---

## Why This Table Matters

The binding table is used by other security features such as:

- **Dynamic ARP Inspection (DAI)**

Without DHCP Snooping, **DAI cannot verify IP-MAC mappings properly**.

So DHCP Snooping becomes a **foundational security feature** in Layer 2 networks.

---

# 5. The "Gotcha": DHCP Option 82 ⚠️

This is one of the **most common troubleshooting issues** engineers face during deployment.

Cisco switches automatically handle **Option 82**, also called the : Relay Agent Information Option


---

## Default Behavior

Cisco switches perform two actions:

### 1️⃣ Option 82 Insertion

When a DHCP request arrives on an untrusted port, the switch automatically **adds Option 82**.

This information may include:

- Switch identifier
- Port identifier

---

### 2️⃣ Option 82 Filtering

If the switch receives a DHCP packet **already containing Option 82 on an untrusted port**, it **drops the packet**.

This can cause unexpected DHCP failures.

---

## Recommended Fix

If your switch is **not acting as a DHCP relay agent**, disable Option 82.

```bash
no ip dhcp snooping information option
```


This resolves most **DHCP Snooping deployment issues**.

---

# 6. Protecting the Switch: Rate Limiting 🚨

Inspecting every DHCP packet requires **CPU processing**.

Attackers may attempt to overload the switch by sending a massive number of DHCP packets.

This is essentially a **DoS attack on the switch itself**.

---

## DHCP Rate Limiting

To protect the switch, you can limit DHCP packet rate per interface.

Example : ip dhcp snooping limit rate 10


Meaning : Maximum 10 DHCP packets per second


---

## Violation Behavior

If the rate exceeds the limit : Port → Error Disabled


The switch automatically shuts down the port to protect itself.

---

## Recovery Options

You can recover the port in two ways.

### Manual Recovery

```bash
shutdown
no shutdown
```

---

### Automatic Recovery

```bash
errdisable recovery cause dhcp-rate-limit
```


The switch will automatically restore the port after a timeout.

---

# 7. Deployment Steps (Step-by-Step) 🧩

A proper DHCP Snooping deployment follows a simple sequence.

---

## Step 1 – Enable DHCP Snooping Globally


```bash
ip dhcp snooping
```


---

## Step 2 – Enable it for Specific VLANs


```bash
ip dhcp snooping vlan <vlan-id>
```


Example : ip dhcp snooping vlan 10



---

## Step 3 – Define Trusted Ports

Apply this to ports connected to DHCP servers or uplinks.

```bash
interface gi0/1
ip dhcp snooping trust
```


---

## Step 4 – Fix Option 82 (If Needed)

```bash
no ip dhcp snooping information option
```


---

## Step 5 – Apply Rate Limiting on Untrusted Ports

Example:

```bash
interface range gi0/2-24
ip dhcp snooping limit rate 10
```


---

# 8. Verification Commands 🔍

After configuration, verification is critical.

---

## Check Global Status

```bash
show ip dhcp snooping
```


Displays:

- Enabled status
- Active VLANs
- Trusted interfaces

---

## View Binding Table

```bash
show ip dhcp snooping binding
```



Shows:

- MAC ↔ IP mapping
- VLAN
- Interface
- Lease time

This command is extremely useful for **troubleshooting IP allocation problems**.

---

# Final Thoughts 🎯

DHCP Snooping is one of the **most important Layer 2 security features** in modern networks.

It protects against:

- Rogue DHCP servers
- DHCP starvation attacks
- Man-in-the-middle attacks
- Invalid DHCP messages

And it provides the **foundation for advanced security mechanisms** like Dynamic ARP Inspection.

In simple terms : DHCP Snooping = DHCP Firewall for your switch


If your network relies on DHCP (which almost every network does), enabling DHCP Snooping should be considered **a baseline security practice**.

---

⭐ **Quick Memory Trick**

Remember the **3 pillars of DHCP Snooping**:

1️⃣ Trusted vs Untrusted Ports
2️⃣ Binding Table
3️⃣ DHCP Packet Inspection


Master these three, and DHCP Snooping will never confuse you again.

---

**Happy Networking! 🚀**


# 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
