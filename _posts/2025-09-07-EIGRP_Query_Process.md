---
title: EIGRP Query Process
date: 2025-09-07 11:11:00 +0000
categories: [Networking_Topics]
tags: [networking, eigrp, networkengineer, cisco, itlab]
---

# 🧠 EIGRP Route Selection & Query Process

---

## 1. ✅ Feasibility Condition (FC)

To qualify as a **Feasible Successor (FS)**, a neighbor's route must meet the **Feasibility Condition**:

Reported Distance (RD) < Feasible Distance (FD)


- Ensures loop-free backup routes.
- FS routes are stored in the **topology table**, not routing table.

---

## 2. 📘 Key Definitions

| Term            | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| **Successor (S)**         | Primary/best path to a destination (added to routing table).             |
| **Feasible Successor (FS)** | Backup route meeting FC (stored in topology table only).                |
| **Feasible Distance (FD)**  | Best known distance from the router to the destination.                |
| **Reported Distance (RD)**  | Distance reported by a neighbor to reach the destination.              |
| **Passive State**          | Route is stable (normal state).                                     |
| **Active State**           | Router is actively searching for a new path by sending queries.     |

---

## 3. 🗺️ Topology Example (Based on Your Diagram)

**Destination:** `192.168.10.0/24`  
**Source Router:** `R1`

| Path   | Next-Hop | Status                          |
|--------|----------|----------------------------------|
| Path 1 | R2       | ✅ **Successor**                 |
| Path 2 | R3       | ❌ Not FS (doesn't meet FC) — not stored   |
| Path 3 | R4       | ✅ **Feasible Successor (FS)**   |

--- 

## 4. 🚨 Failure Scenario Walkthrough

### 🔹 Step 1: Successor (R2) Fails

- FS via R4 **gets promoted** to new **Successor**
- Route remains in **Passive** state (no queries needed)
- Still stable and reachable

---

### 🔹 Step 2: FS (R4) Also Fails

- Now R1 has:
  - ❌ No Successor
  - ❌ No Feasible Successor
- R1 changes route to **Active State**
- Sends **Query messages** to:
  - R3 (even though it wasn’t FS)
  - Any neighbor that may have a valid path

---

## 5. 🔄 EIGRP Query & Reply Mechanism

| Phase | Action |
|-------|--------|
| R1 sets the route to **Active State** | Because no FS is available |
| R1 sends **Query** to neighbors       | “Do you have a route to 192.168.10.0/24?” |
| Neighbors check their own routing/topology tables |
| If they have a valid route → **Reply** |
| If not → Forward Query to their own neighbors |
| R1 waits for **Replies from all queried neighbors** |

---

## 6. ⏱️ SIA (Stuck-in-Active) Process

### ⚠️ What is SIA?

A route becomes **Stuck-in-Active** when a router:
- Sends a query
- But **doesn’t get replies from all neighbors in time**

---

### 🧩 SIA Timer Mechanics

| Timer        | Purpose                                                             |
|--------------|---------------------------------------------------------------------|
| **180 sec total** | Time router waits before declaring route as SIA              |
| **First 90 sec**  | Router may send **SIA-Query** to unresponsive neighbor        |
| **SIA-Reply**     | If neighbor responds to SIA-Query, original 180 sec is **reset** |

### 🔁 So the flow is:

1. **Query sent**
2. If no reply within **90 seconds**, router sends **SIA-Query**
3. If neighbor responds with **SIA-Reply**, route stays active, timer resets
4. If **still no reply** after full **180 seconds**, the route is marked **SIA**
5. Router:
   - Drops the route
   - Possibly resets the neighbor adjacency

---

## 7. ❗ Key Point: Passive vs. Active

| Route State | When It Happens                               |
|-------------|-----------------------------------------------|
| **Passive** | Normal state – route is stable & reachable    |
| **Active**  | Triggered **only** when: no S & no FS         |
|             | Router sends **Query** messages               |

---

## 8. 🧠 Final Recap

| Event                     | R1 Behavior                                                        |
|---------------------------|---------------------------------------------------------------------|
| R2 (Successor) fails      | R4 (FS) becomes new Successor; state remains **Passive**            |
| R4 also fails             | No S/FS → Route becomes **Active**                                  |
| R1 sends Queries          | To all neighbors asking for alternative routes                      |
| No reply in 90 sec        | Sends **SIA-Query**                                                 |
| Reply received            | Route remains Active; timer resets                                  |
| No reply in 180 sec       | Route becomes **Stuck-in-Active**, removed from topology + routing table |
| Neighbor unresponsive     | Adjacency with neighbor may be reset                                |

---
