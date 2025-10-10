---
title: BGP Fundamentals
date: 2025-10-01 04:00:00 +0000
categories: [Networking_Topics]
tags: [bgp_fundamentals, bgp, ccna, ccnp, networking]
---


<h1 align="center">🛰️ BGP Fundamentals – Interview & Beginner Friendly Guide</h1>


<h2 align="center">✅ What is BGP?</h2>


**BGP** stands for **Border Gateway Protocol** – it's the standard exterior gateway protocol (EGP) used to exchange routing **information between autonomous systems (AS)** on the Internet.


<h3>🧠 Key BGP Concepts:</h3>


-   **Layer**: Works at Layer 7 (Application Layer) of the OSI model.
-   **Transport Protocol**: Uses TCP (Transmission Control Protocol).
-   **Port Number**: TCP port 179.
-   **Protocol Type**: Path Vector Protocol (unlike Distance Vector or Link State).
-   **Slow Protocol**: Because of reliability and policy-based nature.


<h3>🔁 Types of BGP</h3>


| Type     | Full Form    | Scope                | Admin Distance |
| -------- | ------------ | -------------------- | -------------- |
| **iBGP** | Internal BGP | Inside same AS       | **200**        |
| **eBGP** | External BGP | Between different AS | **20**         |


>   📌 Note: No auto-discovery or multicast is used in BGP. All neighbors must be configured manually.


<h2 align="center">🌐 BGP Basics</h2>


-   **AS (Autonomous System)**: A group of routers under a single administrative domain.
-   BGP tracks **AS-path** to determine loop-free routing.
-   BGP can form neighborship with:
    -   **Directly connected routers**
    -   **Remotely connected routers** (static routes used to reach them)


<h2 align="center">🛠️ BGP Configuration Syntax</h2>


On Cisco routers (example):

```bash
router bgp 100
  neighbor <IP> remote-as 500
  network <LAN prefixes>
```


<h2 align="center">🔄 BGP Message Types</h2>


| Message Type     | Purpose                                                            |
| ---------------- | ------------------------------------------------------------------ |
| **OPEN**         | Start session, exchange BGP version, AS number, BGP ID, Hold Timer |
| **KEEPALIVE**    | Heartbeat, sent every 60s (⅓ of hold timer 180s)                   |
| **UPDATE**       | Send routing info (NLRI – Network Layer Reachability Info)         |
| **NOTIFICATION** | Send errors, alerts                                                |


<h2 align="center">🧱 TCP/IP Stack Relevance in BGP</h2>


| **Layer**   | **Name**        | **Function**                                                  | **BGP Relevance**                                    | **Port Number Role**                        |
| ----------- | --------------- | ------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------- |
| **Layer 7** | **Application** | Handles application-specific logic (BGP, HTTP, FTP, etc.)     | BGP runs here as a routing application               | **Requests or binds to port 179** (BGP)     |
| **Layer 4** | **Transport**   | End-to-end communication, reliable delivery using TCP         | BGP uses **TCP** for reliable neighbor communication | **Encapsulates source & destination ports** |
| **Layer 3** | **Internet**    | Logical addressing and routing (IP layer)                     | Uses IP addresses to reach BGP peers                 | **No direct involvement with ports**        |
| **Layer 2** | **Data Link**   | Physical addressing and frame delivery (MAC, Ethernet)        | Responsible for hop-to-hop delivery on same link     | **No direct involvement with ports**        |
| **Layer 1** | **Physical**    | Transmits raw bits over physical medium (cables, fiber, etc.) | Underlying physical connectivity between routers     | **No involvement**                          |



<h2 align="center">🔄 BGP Finite State Machine (FSM)</h2>


BGP has several states before a session is fully established:

<h3>🔹 TCP Phase States</h3>

1.  **Idle**
    -   Initial state when `router bgp <asn>` is configured.
    -   On `neighbor <IP> remote-as <asn>`, BGP starts TCP connection (port 179).
    -   Initializes BGP resources and listens for incoming connections.

2.  **Connect**
    -   Starts TCP 3-way handshake (SYN, SYN-ACK, ACK).
    -   If fails, starts **ConnectRetry timer (120s)**.
    -   On failure → goes to **Active**. On success → OpenSent.

3.  **Active**
    -   Tries new TCP connection.
    -   If fails → back to Connect and retries (timer resets).
    -   If successful → move to OpenSent.

>   ⚠️ Frequent transition between Connect/Active indicates TCP issues (ACLs, port 179 blocked, IP reachability).

<h3>🔹 BGP Phase States</h3>


4.  **OpenSent**
    -   Sends **OPEN** message (BGP version, ASN, router ID, Hold Timer, optional capabilities).
    -   If mismatch/error → sends **NOTIFICATION** → back to Idle.
    -   If matched → move to OpenConfirm.

5.  **OpenConfirm**
    -   Waits for **KEEPALIVE** message from peer.
    -   If received → go to Established.
    -   If not received → reset to Idle.

6.  **Established**
    -   BGP session fully up.
    -   Exchange **UPDATE** messages (NLRI, path attributes).
    -   Sends **KEEPALIVE** every **60s (1/3 of Hold Timer 180s)**.
    -   If no UPDATE/KEEPALIVE in hold time → reset to Idle.



<h2 align="center">🧠 Two-Phase Process</h2>


-   **Phase 1: TCP Phase** – Establish transport connection
    -   ➝ Idle → Connect → Active

-   **Phase 2: BGP Phase** – Exchange BGP messages
    -   ➝ OpenSent → OpenConfirm → Established


<h2 align="center">🔁 Quick FSM Flow Summary</h2>


```bash
Idle
 ├─▶ Connect (start TCP handshake)
 │    ├─▶ success → OpenSent
 │    └─▶ fail → Active
 │               ├─▶ success → OpenSent
 │               └─▶ fail → Connect (retry)
OpenSent
 └─▶ OpenConfirm (if OPEN accepted)
       └─▶ Established (if KEEPALIVE received)
```

>   ✅ In Established, routes are exchanged via UPDATEs, and session health is maintained via KEEPALIVEs. If any error or timer expires → back to Idle.



<h2 align="center">🔁 BGP State Flow Summary</h2>


```bash
Idle → Connect → Active → OpenSent → OpenConfirm → Established
```

>   Each transition depends on **TCP success/failure and BGP message exchange**.


<h2 align="center">🧩 Inter-AS BGP Example (From Diagrams)</h2>

-   R1 (AS 100) ↔ R2 (AS 200): eBGP
-   R2 → R3: Static route (to reach remote peer)
-   R3 ↔ R4 (AS 200): iBGP
-   R4 advertises internal routes to ISP 1/2 using BGP.


<h2 align="center">🔗 Redistribution and Routing</h2>


-   Common redistribution from:
    -   **OSPF** → Admin Distance: 110
    -   **EIGRP** → Admin Distance: 90
    -   **Static** → Admin Distance: 1
-   **BGP routes** can be redistributed to IGPs (OSPF, EIGRP), and vice versa.


<h2 align="center">🧠 Additional Key Terms</h2>


-   **NLRI** (Network Layer Reachability Info): The actual IP prefixes being advertised.
-   **AS Path**: Tracks all AS numbers a route has passed through. Used for loop prevention.
-   **Hold Timer**: Typically set to 180 seconds.
-   **Keepalive Timer**: Sent every 60 seconds.


<h2 align="center">📌 Interview Tips</h2>


-   Know all **BGP FSM** states and transitions.
-   Be able to explain **TCP 3-way handshake** in the context of BGP.
-   Understand difference between **iBGP vs eBGP**.
-   Remember BGP does **not use multicast or auto-discovery**.
-   Know default **Administrative Distances**:
    -   eBGP = 20
    -   iBGP = 200
-   Be able to draw or describe **AS-level topologies**.


<h2 align="center">✅ Conclusion</h2>


BGP is the backbone of internet routing. While it's complex, understanding the **message types, state machine**, and **configuration basics** can set a strong foundation for deeper learning and acing interviews.




## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)

