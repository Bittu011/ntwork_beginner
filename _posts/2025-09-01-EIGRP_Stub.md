---
title: EIGRP Stub Routing
date: 2025-09-01 04:00:00 +0000
categories: [Networking_Topics]
tags: [eigrp_stub_routing, eigrp, ccna, ccnp, networking]
---


<h1 align="center">🛰️ EIGRP Stub Routing: Fortifying the Network Edge and Accelerating Convergence</h1>

>   Category: EIGRP | Level: Intermediate–Advanced | Use Case: WAN Optimization & Query Suppression



<h2 align="center">🚀 Introduction: Why Stub Routing Matters</h2>


Enhanced Interior Gateway Routing Protocol (EIGRP) is widely respected for its fast convergence, powered by its **Diffusing Update Algorithm (DUAL)**.

But in **large-scale** or **hub-and-spoke networks**, this speed can come at a cost — EIGRP’s query mechanism can generate excessive control-plane traffic and CPU utilization during route recalculations.

That’s where **EIGRP Stub Routing** becomes the hidden weapon. It:

-   Restricts query scope
-   Prevents unnecessary transit routing
-   Enhances convergence and WAN stability

In essence, it protects your network’s core by reducing unnecessary computations from remote edge routers.


<h2 align="center">⚙️ Part 1: The Problem — “The Query Storm”</h2>


When a route disappears in a standard EIGRP topology:

1.  The router detecting the loss checks for a Feasible Successor (FS).
2.  If no FS exists, the route goes Active, triggering a query process.
3.  The router sends QUERY packets to all EIGRP neighbors asking for an alternate route.


This process, known as Diffusing Computation, can cause major delays:

-   If any neighbor doesn’t reply in time (≈90 seconds), the route becomes Stuck-In-Active (SIA).
-   An SIA condition can tear down the neighbor adjacency, increasing convergence time.


<h3>📉 Real-World Impact:</h3>

In hub-and-spoke WAN designs, a query sent from the hub to multiple spoke routers causes query flooding, leading to CPU strain and potential route instability.


<h3>✅ Stub Solution:</h3>

EIGRP Stub Routing limits query propagation to prevent edge routers from participating in such diffusing computations.


<h2 align="center">🧩 Part 2: What is an EIGRP Stub Router?</h2>


An **EIGRP Stub Router** is a device configured to tell its neighbors “Don’t query me for alternate paths.”

When configured, the router:


-   Adds a **Stub flag** in its **Hello packets**.
-   Neighbors record this flag in their **EIGRP neighbor table**.
-   Hub routers **avoid sending queries** to the stub when routes go active.



<h3>🔑 Benefits of Stub Routers</h3>


| Benefit                      | Description                                                                                               |
| ---------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Faster Convergence**       | Eliminates unnecessary queries to edge routers, reducing SIA chances.                                     |
| **Prevents Transit Routing** | Stub routers cannot forward traffic between two core peers — ensuring they’re not used as transit points. |
| **Resource Optimization**    | Reduces CPU load and EIGRP update traffic, especially over slow WAN links (like T1).                      |


<h2 align="center">🛠️ Part 3: EIGRP Stub Configuration and Behavior</h2>


By default, the `eigrp stub` command advertises only connected and summary routes


<h3>🔧 Command Syntax</h3>


```bash
Router(config-router)# eigrp stub {connected | static | redistributed | summary | receive-only}
```

<h3>🧠 Option Breakdown</h3>


| Option            | Behavior                                                | Technical Insight                                                        |
| ----------------- | ------------------------------------------------------- | ------------------------------------------------------------------------ |
| **connected**     | Advertises directly connected routes. *(Default)*       | Shares networks originated in the local EIGRP AS.                        |
| **summary**       | Advertises summary routes. *(Default)*                  | Controls query scope and promotes route aggregation.                     |
| **redistributed** | Advertises redistributed routes from other protocols.   | Marks routes as External (EIGRP Type 5).                                 |
| **static**        | Advertises static routes configured on the stub router. | Must be manually specified.                                              |
| **receive-only**  | Router only receives routes — advertises none.          | Cannot be combined with other options; used for fully isolated branches. |


<h3>🧩 Example (IPv4 Classic Mode)</h3>

```bash
R1(config)# router eigrp 100
R1(config-router)# eigrp stub connected summary
```


<h3>🧩 Example (EIGRP Named Mode)</h3>

```bash
R1(config)# router eigrp CORP
R1(config-router)# address-family ipv4 unicast autonomous-system 100
R1(config-router-af)# eigrp stub connected summary
```


<h3>🧩 Example (IPv6 EIGRP)</h3>

```bash
R1(config)# ipv6 router eigrp 100
R1(config-router)# eigrp stub connected summary
```



<h3>🔍 Verification Commands</h3>

| Command                          | Purpose                                             |
| -------------------------------- | --------------------------------------------------- |
| `show ip eigrp neighbors detail` | Displays neighbor status and stub flag recognition. |
| `show ip protocols`              | Confirms EIGRP stub configuration.                  |
| `show ip eigrp topology`         | Verifies route advertisements and query scope.      |


<h2 align="center">🧭 Part 4: The EIGRP Stub Site — Advanced WAN Optimization</h2>


In real-world topologies, a stub router may still have **downstream routers** on its LAN side.

If configured as a regular stub, those downstream networks won’t be advertised to the core — causing **reachability loss**.


To solve this, Cisco introduced the EIGRP Stub Site feature (available only in EIGRP Named Mode).


<h3>⚙️ How EIGRP Stub Site Works</h3>


1.  WAN Interfaces: Identified as the boundary toward the core network.
2.  LAN Interfaces: Continue to exchange EIGRP routes with downstream routers.
3.  Transit Prevention: Routes tagged with the stub-site identifier are not re-advertised out other WAN interfaces.


**Thus, the router**:

-   Acts as a stub only toward WAN peers.
-   Maintains normal routing with LAN peers.
-   Prevents spoke-to-spoke routing through the site.



<h3>🧩 Example Configuration</h3>


```bash
router eigrp EIGRP-NAMED
 address-family ipv4 unicast autonomous-system 100
  af-interface Serial1/0
   stub-site wan-interface
  exit-af-interface
  eigrp stub-site 100:1
```



**Explanation**:

-   `stub-site wan-interface`: Defines Serial1/0 as the WAN edge.
-   `eigrp stub-site 100:1`: Creates a unique identifier for this site.
-   Ensures no transit routing occurs between WANs.


<h2 align="center">🧱 Design Considerations</h2>


| Scenario                                            | Recommended Stub Type               |
| --------------------------------------------------- | ----------------------------------- |
| Remote site with only LAN users                     | Standard stub (`connected summary`) |
| Remote site with redistributed static routes        | Stub with `static` option           |
| Remote site with local redistribution               | Stub with `redistributed`           |
| Spoke router with multiple downstream routers       | Stub site configuration             |
| Device for monitoring or backup (no advertisements) | `receive-only` stub                 |


<h2 align="center">⚡ Key Takeaways</h2>

✅ **EIGRP Stub Routing** limits query scope and protects edge routers from participating in route computations.
✅ **It prevents Stuck-In-Active (SIA)** events by stopping unnecessary query propagation.
✅ **Default stub options**: Connected + Summary routes.
✅ **EIGRP Stub Site** adds control for dual-interface routers (WAN + LAN) without breaking reachability.
✅ Critical for **hub-and-spoke WAN, DMVPN**, and **remote branch** topologies.


<h2 align="center">🧠 Interview & Revision Quickfire</h2>

| Question                                           | Answer                                                        |
| -------------------------------------------------- | ------------------------------------------------------------- |
| What is the main purpose of EIGRP Stub Routing?    | To limit query propagation and prevent SIA events.            |
| Default stub advertisement types?                  | Connected and summary routes.                                 |
| Command to configure stub?                         | `eigrp stub [options]`                                        |
| What does “receive-only” mean?                     | Router does not advertise any routes, only receives.          |
| What is EIGRP Stub Site used for?                  | To allow LAN EIGRP peers while maintaining WAN stub behavior. |
| How to verify stub status?                         | `show ip eigrp neighbors detail`                              |
| Does a stub router participate in transit routing? | No — it cannot forward traffic between EIGRP peers.           |



<h2 align="center">🏁 Conclusion</h2>

EIGRP Stub Routing is the **shield for your network edges** — it contains route queries, speeds up convergence, and stabilizes EIGRP in large or distributed environments.


By integrating **Stub and Stub Site** configurations intelligently, you achieve:


-   💨 Faster failover
-   🧠 Reduced query overhead
-   🧩 Scalable and stable EIGRP domains

This makes it a must-know concept for every Network Engineer, EIGRP designer, or interviewer evaluating real-world routing design knowledge.




## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)

