---
title: Port Security
date: 2026-02-20 04:00:00 +0000
categories: [CCNA]
tags: [Port Security, CCNA]
---

# Port Security – The Gatekeeper of Your Switch  
### A Creative Deep Dive & Revision Masterclass

Imagine your network switch as a **high-security building**.

Every device—laptop, printer, IP phone, or server—is a **visitor** trying to enter that building. The switch keeps a list of everyone who has entered before. In networking terms, this list is called the **MAC Address Table**.

Normally, switches happily learn MAC addresses from any device that sends traffic. But what happens if **someone abuses that trust?**

This is where **Port Security** becomes the **gatekeeper of your network**.

Port Security is a **Layer 2 security feature on Cisco switches** that protects the MAC address learning process by:

- Limiting the number of MAC addresses allowed on a port
- Allowing only trusted devices
- Taking action when suspicious activity occurs

This guide will help you **learn, revise, and master Port Security** step by step.

---

# Why Port Security Exists  
## Mitigating Layer 2 Attacks

Without Port Security, a switch learns **as many MAC addresses as its table can hold**—sometimes thousands.

Attackers can exploit this behavior using several techniques.

---

## MAC Flooding Attack

In a **MAC Flooding attack**, an attacker sends thousands of frames with **spoofed (fake) MAC addresses**.

### What Happens?

1. The switch learns each fake MAC address.
2. The MAC address table eventually becomes **full**.
3. The switch can no longer store new entries.

When this happens, the switch begins behaving like a **hub**.

Instead of forwarding frames intelligently, it **floods traffic out of all ports**.

### Result

- Attackers can **capture network traffic**
- Sensitive data can be intercepted
- Network security is compromised

---

## DHCP Exhaustion Attack

Another Layer 2 attack is **DHCP Exhaustion**.

In this attack, the attacker sends **multiple DHCP DISCOVER messages** using fake MAC addresses.

Each request consumes an IP address from the DHCP pool.

### Result

Eventually:

- The DHCP pool becomes **empty**
- Legitimate users **cannot obtain an IP address**
- Users lose connectivity to the network

---

## The Role of Port Security

Port Security helps prevent these attacks by:

- Limiting how many MAC addresses can connect to a port
- Allowing only **approved devices**
- Blocking suspicious activity

Think of it as a **security guard checking every device entering the network**.

---

# Foundation: Basic Configuration & Requirements

Before enabling Port Security, the switch port must be configured in **manual mode**.

Port Security **cannot be enabled on dynamic ports that use DTP (Dynamic Trunking Protocol).**

You must manually configure the port as either:

- Access mode
- Trunk mode

---

## Basic Port Security Configuration

```
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
```

Once enabled, the switch begins monitoring and controlling MAC addresses on that port.

---

## Verification Command

To check the status of Port Security:

```
show port-security interface fa0/1
```

This command displays:

- Port status
- Violation mode
- Maximum allowed MAC addresses
- Currently learned secure MAC addresses

---

# Managing the MAC Address Limit

Port Security allows administrators to define **how many devices can connect to a port**.

---

## Default Limit

By default:

```
Maximum MAC addresses per port = 1
```

This works perfectly when:

- A single device connects to the port
- Example: a desktop computer

---

## Increasing the Limit

Some situations require **multiple devices** on the same port.

Examples include:

- An **IP phone with a PC connected behind it**
- A **switch uplink**
- Virtualized environments

In these cases, increase the limit:

```
Switch(config-if)# switchport port-security maximum 3
```

Now the port can learn **three secure MAC addresses**.

---

# The Three Types of Secure MAC Addresses

Port Security allows MAC addresses to be secured in **three different ways**.

---

## 1. Dynamic Secure MAC Addresses

These addresses are **learned automatically from traffic**.

### Characteristics

- Stored in the **MAC address table**
- Not saved in the switch configuration
- Lost if:
  - The switch reboots
  - The port goes down

This method requires **no manual configuration**, but it is temporary.

---

## 2. Static Secure MAC Addresses

Static MAC addresses are **manually configured by the administrator**.

Example configuration:

```
Switch(config-if)# switchport port-security mac-address 00AA.BB11.CC22
```

### Characteristics

- Stored in the **running configuration**
- Persistent across:
  - Reboots
  - Interface shutdowns
- Ideal for **critical infrastructure devices**

---

## 3. Sticky Secure MAC Addresses

Sticky MAC addresses combine the **advantages of dynamic and static learning**.

Enable sticky learning using:

```
Switch(config-if)# switchport port-security mac-address sticky
```

### How Sticky MAC Works

1. The switch dynamically learns MAC addresses from traffic.
2. The switch automatically adds those MAC addresses to the **running configuration**.

### Advantages

- No manual configuration required
- MAC addresses persist after reboot

Because of this balance, **sticky MAC is commonly used in production networks**.

---

# Secure MAC Address Aging

Over time, devices may change or move to different switch ports.

Port Security includes **aging mechanisms** to remove old MAC entries.

---

## Default Aging Behavior

By default:

```
Aging time = 0 minutes
```

This means **aging is disabled**, and secure MAC addresses remain indefinitely.

---

## Absolute Aging

With **absolute aging**, the MAC address is removed after a fixed amount of time.

Example:

```
Aging Time = 60 minutes
```

After one hour, the MAC address entry is removed **even if the device is still active**.

---

## Inactivity Aging

With **inactivity aging**, the timer resets whenever traffic is received from the device.

If the device becomes inactive for the configured period, the entry is removed.

This works similarly to **normal MAC table aging**.

---

## Static MAC Aging

Static MAC addresses normally **do not age out**.

However, aging can be enabled with:

```
switchport port-security aging static
```

This allows manually configured addresses to expire after a defined period.

---

# Violation Modes – Choosing the Punishment

When a **non-secure MAC address appears** or the **maximum MAC limit is exceeded**, the switch triggers a violation.

Administrators can choose **how the switch reacts**.

---

## 1. Shutdown Mode (Default)

This is the **strictest security policy**.

Actions taken:

- Violating frames are dropped
- The port enters **error-disabled state**
- Logs and SNMP alerts are generated

Result: the port is **completely disabled** until manually or automatically recovered.

---

## 2. Restrict Mode

Restrict mode blocks unauthorized devices but keeps the port active.

Actions:

- Violating frames are dropped
- Violation counter increases
- Log and SNMP messages are generated
- The port remains operational

This provides **security without fully disabling the interface**.

---

## 3. Protect Mode

Protect mode is the **least aggressive option**.

Actions:

- Violating frames are silently dropped
- No logs are generated
- No counters are incremented
- Port remains active

It prevents unauthorized access but **does not alert administrators**.

---

## Violation Mode Comparison

| Feature | Shutdown | Restrict | Protect |
|-------|--------|--------|--------|
| Discard violating frames | Yes | Yes | Yes |
| Error-disable port | Yes | No | No |
| Increment violation counter | Yes | Yes | No |
| Generate log/SNMP message | Yes | Yes | No |

---

# Port Recovery – Bringing the Network Back

If a port enters **secure-shutdown (error-disabled state)** due to a violation, it must be recovered.

There are two recovery methods.

---

## Manual Recovery

An administrator manually resets the interface.

```
Switch(config)# interface fa0/1
Switch(config-if)# shutdown
Switch(config-if)# no shutdown
```

This resets the port and restores connectivity.

---

## Automatic Recovery (ErrDisable Recovery)

Cisco switches can automatically recover ports after a violation.

Enable automatic recovery:

```
Switch(config)# errdisable recovery cause psecure-violation
```

### Default Recovery Timer

```
300 seconds (5 minutes)
```

After this time, the switch attempts to **automatically re-enable the port**.

---

# Essential Revision Commands

These commands are essential when **troubleshooting or reviewing Port Security configuration**.

---

## Show Port Security Summary

```
show port-security
```

Displays a summary of **all secured ports**.

---

## View Secure MAC Addresses

```
show port-security address
```

Shows:

- SecureConfigured MAC addresses
- SecureDynamic MAC addresses

---

## View Secure Entries in the MAC Table

```
show mac address-table secure
```

Secure MAC addresses appear as **STATIC entries**.

---

## Check ErrDisable Recovery Status

```
show errdisable recovery
```

Confirms whether **automatic recovery is enabled for Port Security violations**.

---

# Final Thoughts

Port Security is one of the **simplest yet most powerful security mechanisms** available on Layer 2 switches.

By controlling which devices are allowed to connect to a port, it protects networks from:

- MAC flooding attacks
- DHCP exhaustion attacks
- Unauthorized device access

In real-world enterprise networks, administrators commonly combine:

- **Sticky MAC learning**
- **Appropriate maximum limits**
- **Restrict violation mode**

This combination provides a balance between **security, automation, and operational stability**.

Think of Port Security as the **bouncer at the door of your network** — only trusted devices get inside.

---

# Quick Revision Cheat Sheet

### Enable Port Security

```
switchport mode access
switchport port-security
```

### Set Maximum MAC Addresses

```
switchport port-security maximum 2
```

### Enable Sticky MAC Learning

```
switchport port-security mac-address sticky
```

### Configure Violation Mode

```
switchport port-security violation restrict
```

### Enable Automatic Recovery

```
errdisable recovery cause psecure-violation
```

---

**Mastering Port Security is an essential step toward building secure and resilient enterprise networks.**

Happy Networking!

# 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)

