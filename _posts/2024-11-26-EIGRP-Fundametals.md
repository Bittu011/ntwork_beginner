---
title: EIGRP Fundamentals
date: 2024-11-26 05:00:00 +0000
categories: [Networking_Topics, CCNA]
tags: [eigrp,ccna, ccnp, cisco]
---

## Why EIGRP matters

- Supports **unequal-cost load balancing** using the variance feature, unlike most IGPs, allowing efficient use of diverse links.
- Converges rapidly by leveraging the **Diffusing Update Algorithm (DUAL)** and precomputed loop‑free backups called feasible successors.

## Core algorithm: DUAL

- **DUAL** tracks route states, calculates best paths, and coordinates queries/replies to ensure loop-free convergence during changes.
- If a successor fails and a feasible successor exists, the route switches instantly without running a full diffusion, minimizing downtime.

## Key tables

- **Neighbor table** holds directly connected EIGRP peers discovered via Hellos; **topology table** stores all learned routes with their metrics and candidates; **routing table** installs only chosen successors (and additional paths per variance).
- The topology table is DUAL’s workspace, tracking each destination’s successors, feasible successors, and metrics for rapid failover.

## Distances and metrics

- **Reported/Advertised Distance (RD/AD):** the metric a neighbor claims to the destination.
- **Feasible Distance (FD):** the local total metric to the destination via a given neighbor; the lowest FD determines the successor and is used for variance calculations.

## Fast convergence behavior

- With an FS present, convergence is instantaneous upon successor loss; without an FS, DUAL diffuses queries to find a new loop‑free path.
- Triggers for DUAL include successor loss without an FS, topology changes, or received queries that necessitate recomputation.

## Blog-ready definitions

- **DUAL:** Loop-free route computation and rapid reconvergence engine for EIGRP.
- **Variance:** Multiplier to allow unequal-cost load balancing by admitting additional FS paths whose FD is within the threshold.
- **Successor route:** The path A → B → X with the lowest cost.
- **Successor:** Router B (next-hop from A).
- **Feasible Distance:** Total cost from A to X via B.
- **Reported Distance:** Cost from B to X (reported by B).
- **Feasibility condition:** Is the cost from B to X (RD) less than A's own best total cost to X (FD)?
- **Feasible successor:** Say Router C reports a path to X with RD < A’s FD—A can keep C as a backup route to X.

---

## Multiple processes

- A single router can run several EIGRP processes simultaneously; each process is scoped to an **AS number** and maintains its own neighbor, topology, and routing tables.
- Processes in different ASNs do not share routes by default; they behave as separate routing domains unless redistribution is configured.

## Autonomous systems

- An EIGRP **autonomous system** is a common routing domain for routers that share metrics and exchange updates; do not confuse it with an Internet BGP AS.
- Routers must use the same EIGRP ASN to form adjacencies; the ASN is configured with router eigrp ASN in classic mode or under address-family in named mode.

## Route transfer between ASNs

- EIGRP does not automatically move prefixes across different EIGRP ASNs; implement **redistribution** between processes to leak or share routes intentionally.
- When redistributing, plan summarization and filtering to avoid accidental leaks or suboptimal paths, similar in spirit to controlling leaks in other protocols.

## Protocol-dependent modules (PDMs)

- **PDMs** give EIGRP multiprotocol capability; each PDM handles send/receive, DUAL notifications, and installing routes for its specific protocol.
- Current implementations focus on **IPv4 and IPv6** PDMs, while historic PDMs included AppleTalk and IPX.

---

EIGRP maintains a dedicated topology table per AS/address family that DUAL uses to compute loop-free successors and feasible successors for rapid convergence.

## What it stores

- The table lists every known destination prefix within the EIGRP domain along with next-hop candidates learned from neighbors.
- For each prefix, it records the neighbors that advertised it, their reported/advertised distance, and hop count metadata.
- It also retains per-path metric components used in EIGRP’s composite metric: minimum bandwidth, cumulative delay, and optionally load and reliability.

## Why it’s different

- Unlike pure distance-vector protocols, EIGRP’s topology table lets DUAL precompute loop-free alternates, enabling immediate failover when a successor fails.
- Only the best path (successor) is installed in the routing table by default; backups remain in the topology table unless features like FRR preinstall them.

---

EIGRP uses its own Reliable Transport Protocol (RTP) over IP protocol number 88 to deliver routing messages, multicasting to 224.0.0.10 (MAC 01:00:5E:00:00:0A) when possible and unicasting when reliability demands it.

## Neighbor adjacencies

- Neighbors are discovered with periodic Hello packets and tracked in the adjacency table; full topology exchange occurs only when the adjacency forms, after which only incremental updates are sent.
- Loss of acknowledgments or missed Hellos triggers recovery: EIGRP may shift from multicast to unicast per-neighbor and can reset the adjacency after retry limits.

## Packet types

- Hello: used for discovery and liveliness detection; sent unreliably via multicast, no ACK required.
- Update: carries routing and reachability; sent reliably via multicast or unicast depending on scope.
- Query: asks neighbors for alternate paths during convergence; sent reliably, typically multicast.
- Reply: response to a Query; sent reliably via unicast.
- ACK: acknowledgment of reliable packets; always unicast and itself does not require acknowledgment.

## Multicast details

- Default IPv4 multicast group for EIGRP control traffic is 224.0.0.10, which maps on Ethernet to MAC 01-00-5E-00-00-0A; only hosts joined to this group process the frames.
- Routers fall back to unicast when a neighbor fails to acknowledge reliable multicasts, preventing unnecessary retransmissions to other neighbors

## RTP reliability mechanics

- Every reliable EIGRP packet carries a nonzero sequence number; the receiver must return an ACK echoing that sequence for ordered delivery.
- If ACKs are not received before the retransmission timeout (RTO) derived from SRTT, EIGRP retransmits; after up to 16 retries, the neighbor is reset.

---

EIGRP forms a neighbor adjacency before installing routes in the RIB; on receiving Hellos, routers attempt to peer but only succeed when key parameters match.

## Must-match parameters

- Metric formula **K values** must be identical on both routers; any mismatch prevents adjacency and should be changed consistently AS-wide.
- The interfaces’ **primary IP subnets** must match; if one side uses a different primary network (even with a matching secondary), neighbors do not form.
- The **Autonomous System Number (ASN)** for the EIGRP process/address-family must be the same on both peers.
- **Authentication settings** (mode and keys) must match across neighbors if enabled.


## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
