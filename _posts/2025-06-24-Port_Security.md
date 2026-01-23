---
title: Port Security
date: 2025-06-24 04:00:00 +0000
categories: [Networking_Topics]
tags: [port_security, ccna, ccnp, cisco, network_topics]
---


<h1 align="center">Port Security – Layer 2 Security</h1>

<h2 align="center">1. Introduction to Port Security</h2>

Port Security is a Layer 2 security feature used on switches to **control access to a switch port** by restricting the number and type of MAC addresses that can connect.

- Primary Goal: **Prevent unauthorized devices** from accessing the network.
- Works on: **Access ports** (commonly), but can also be applied to trunk ports.
- Helps mitigate: **MAC flooding attacks, unauthorized device connections, CAM table overflow**.


<h2 align="center">2. Why Port Security?</h2>

Without port security, any device can be plugged into a switch port, potentially leading to:

- Unauthorized access to the LAN.
- Attackers injecting malicious traffic.
- CAM table overflow (MAC flooding) – leading to DoS conditions.

Port Security **limits or specifies** which MAC addresses can use a port, reducing attack surface.

<h2 align="center">3. Key Concepts</h2>

- **Secure MAC Address**: A MAC address allowed by port security.
- **Maximum MAC Addresses**: Limit of secure MACs allowed per port.
- **Sticky MAC Addressing**: Dynamically learns and saves MAC addresses to the running/startup config.
- **Violation Modes**: Determines switch behavior when unauthorized MACs are detected.


<h2 align="center">4. Port Security Configuration Steps</h2>

### Step 1: Enable Port Security

```bash
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
```

### Step 2: Configure Maximum MAC Addresses

```bash
Switch(config-if)# switchport port-security maximum 2
```

- Limits the port to 2 MAC addresses.

### Step 3: Configure Sticky MAC Addresses

```bash
Switch(config-if)# switchport port-security mac-address sticky
```

### Step 4: (Optional) Manually Configure Allowed MAC

```bash
Switch(config-if)# switchport port-security mac-address 0011.2233.4455
```

### Step 5: Configure Violation Mode

```bash
Switch(config-if)# switchport port-security violation {protect | restrict | shutdown}
```

<h2 align="center">5. Violation Modes in Detail</h2>

| Mode         | Behavior                                                                                    | Logging/Alert | Port State |
| ------------ | ------------------------------------------------------------------------------------------- | ------------- | ---------- |
| **Protect**  | Drops unauthorized MAC frames silently.                                                     | No            | Remains up |
| **Restrict** | Drops unauthorized MAC frames **and generates log/SNMP trap, increments violation counter** | Yes           | Remains up |
| **Shutdown** | (Default) Puts the port into **err-disabled state**, requires manual/no-shut to recover.    | Yes           | Down       |


<h2 align="center">6. Verification Commands</h2>

```bash
Switch# show port-security interface fa0/1
```

Displays port security configuration and status.

```bash
Switch# show port-security
```

Shows global port security status.


```bash
Switch# show mac address-table secure
```
Lists secure MAC addresses.



<h2 align="center">7. Recovery from Violation (Err-disabled Port)</h2>

If a port goes down due to violation (default shutdown mode):

```bash
Switch(config)# errdisable recovery cause psecure-violation
Switch(config)# errdisable recovery interval 30
```

Or manually bring it back:

```bash
Switch(config)# interface fa0/1
Switch(config-if)# shutdown
Switch(config-if)# no shutdown
```



<h2 align="center">8. Real-World Use Cases</h2>

- **Enterprise networks**: Preventing rogue devices from plugging in.
- **Campus environments**: Ensuring only authorized PCs/students connect.
- **Data centers**: Restricting servers/NICs from being swapped without permission.


<h2 align="center">9. Common Interview Questions</h2>

- What is Port Security and why is it used?
- Explain the difference between Protect, Restrict, and Shutdown violation modes.
- What is Sticky MAC and why is it useful?
- Can Port Security be applied on trunk ports?
- How do you recover a port in err-disabled state due to a violation?


<h2 align="center">10. Quick Revision Cheat Sheet</h2>

```bash
🔹 Enable Port Security:
Switch(config-if)# switchport port-security

🔹 Maximum MACs:
Switch(config-if)# switchport port-security maximum 2

🔹 Sticky MAC:
Switch(config-if)# switchport port-security mac-address sticky

🔹 Manual MAC:
Switch(config-if)# switchport port-security mac-address 0011.2233.4455

🔹 Violation Modes:
protect → drops silently
restrict → drops + logs
shutdown → err-disabled (default)

🔹 Verification:
show port-security
show port-security interface fa0/1
show mac address-table secure
```



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
