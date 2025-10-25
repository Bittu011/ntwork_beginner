---
title: BGP Path Selection - Technical Notes
date: 2025-10-20 04:00:00 +0000
categories: [CCNA]
tags: [ccna, ntworkengineer, ccnp enarsi, ccnp, bgp]
---

# 🧭 BGP Path Selection – Complete Technical Notes

**For Interviewers, Learners, and Network Engineers**

---

## I. Beginner Level – Core Concepts & Attributes

### 📘 What is BGP Path Selection?

BGP (**Border Gateway Protocol**) is an *Exterior Gateway Protocol (EGP)* used to exchange routing information between Autonomous Systems (AS).  
Unlike OSPF or EIGRP, BGP is **policy-based**, not **metric-based** — it uses multiple path attributes for decision-making.

---

### 🎯 Key Objectives of BGP Path Selection

BGP determines the best route using these **five primary attributes**:

| # | Attribute | Description |
|---|------------|-------------|
| 1 | **Weight** | Cisco proprietary; higher value preferred |
| 2 | **Local Preference** | Influences routing within the AS; higher preferred |
| 3 | **AS Path** | Fewer AS hops preferred |
| 4 | **Origin** | Route source indicator; IGP > Incomplete |
| 5 | **MED (Multi-Exit Discriminator)** | Lower value preferred |

---

### 🧠 Mnemonic for Attribute Order

> **“We Love Oranges As Oranges Mean Pure Refreshments”**

| Mnemonic | Attribute | Preference |
|-----------|------------|------------|
| We | Weight | Higher is better |
| Love | Local Preference | Higher is better |
| Oranges | Originated Locally | Locally originated routes preferred |
| As | AS Path | Shorter is better |
| Oranges | Origin Code | IGP (i) > Incomplete (?) |
| Mean | MED | Lower is better |
| Pure | Path Type | eBGP > iBGP |
| Refreshments | Router ID | Lower is better |

---

### ⚙️ Attribute Highlights

| Attribute | Type | Scope | Default | Notes |
|------------|------|--------|----------|-------|
| **Weight** | Cisco proprietary | Local to router | 0 (learned), 32768 (local) | Not advertised to peers |
| **Local Preference** | Well-known discretionary | Entire AS | 100 | Shared within iBGP peers |
| **AS Path** | Well-known mandatory | Global | — | Counts AS hops |
| **Origin Code** | Well-known mandatory | Global | — | `i` (IGP) > `?` (Incomplete) |
| **MED** | Optional non-transitive | Between ASes | — | Lower preferred, not passed beyond neighbor AS |

---

## II. Intermediate to Advanced Level – Technical Criteria & Configuration

### 🔖 A. BGP Attribute Classification

| Type | Description | Examples |
|------|--------------|-----------|
| **Well-Known Mandatory** | Must exist in all updates, understood by all vendors | AS Path, Origin |
| **Well-Known Discretionary** | Understood by all vendors, may not appear | Local Preference |
| **Optional Transitive** | May not be understood, but carried across ASes | Communities |
| **Optional Non-Transitive** | Not understood or carried beyond neighbor AS | MED, Weight (Cisco) |

---

### ⚡ B. Full BGP Path Selection Criteria (Top–Down)

BGP always follows the same decision order.  
If a tie occurs at one level, it continues to the next.

| Step | Criteria | Preference Rule |
|------|-----------|-----------------|
| 1 | **Next-Hop Reachability** | Must be reachable |
| 2 | **Highest Weight** | Higher wins |
| 3 | **Highest Local Preference** | Higher wins |
| 4 | **Locally Originated Route** | Prefer self-originated |
| 5 | **Shortest AS Path** | Fewer AS hops preferred |
| 6 | **Lowest Origin Code** | IGP (i) > Incomplete (?) |
| 7 | **Lowest MED** | Lower wins |
| 8 | **eBGP over iBGP** | External routes preferred |
| 9 | **Lowest IGP Metric to Next Hop** | Lower metric preferred |
| 10 | **Oldest Path** | More stable route preferred |
| 11 | **Lowest BGP Router ID** | Smaller ID preferred |
| 12 | **Shortest Cluster List** | Used with Route Reflectors |
| 13 | **Lowest Neighbor IP** | Used as final tiebreaker |

---

### 🧩 C. Configuration & Attribute Manipulation Rules

Attributes are applied in the **direction of control-plane updates**.

| Attribute | Direction | Scope / Impact | Example Command |
|------------|------------|----------------|------------------|
| **Weight** | Inbound only | Local router only | `set weight 1000` |
| **Local Preference** | Inbound from eBGP / Outbound to iBGP | Entire AS | `set local-preference 200` |
| **AS Path** | Inbound / Outbound | Makes path appear longer | `set as-path prepend 100 100 100` |
| **Origin** | Inbound / Outbound | Affects route preference | `set origin incomplete` |
| **MED** | Outbound to eBGP | Affects neighbor’s path selection | `set metric 50` |

---

### 🧱 D. Configuration Differences – Cisco IOS XE vs IOS XR

| Platform | Policy Mechanism | Matching Method | Implicit Deny Behavior | Notes |
|-----------|------------------|------------------|------------------------|-------|
| **Cisco IOS XE (XZ)** | Route Map | `match ip address <ACL>` | Must include a final `permit` for unmatched routes | Common in classic IOS/IOS-XE |
| **Cisco IOS XR (XR)** | Route Policy (RPL) | `if destination in <prefix-set>` | Must include `else pass` | Modular and reusable |
| **XR eBGP Default** | — | — | Routes blocked unless explicit `pass` | Policy required for eBGP peers |

---

### 🧮 BGP Path Manipulation Summary

| Scenario | Attribute to Tune | Direction | Use Case |
|-----------|------------------|------------|-----------|
| Prefer local exit within AS | Local Preference | Inbound | Internal traffic control |
| Prefer one ISP over another | Weight | Inbound | Local outbound path selection |
| Make path less preferred | AS Path Prepend | Outbound | Influence inbound traffic |
| Influence neighbor’s choice | MED | Outbound | Encourage entry via certain link |
| Modify origin type | Origin | Outbound | Adjust route preference |

---

### 🧠 Real-World Key Points

- **Weight** – Local-only, not shared.  
- **Local Preference** – Shared within AS via iBGP.  
- **AS Path Prepending** – Used to influence *inbound* traffic.  
- **MED** – Only applies between directly connected ASes.  
- **eBGP > iBGP** – Always preferred when all else is equal.  
- **Router ID** – Final tie-breaker.

---

## III. Quick Revision & Interview Shortcut

| Step | Attribute | Preferred |
|------|------------|------------|
| 1 | Next-Hop Reachability | Must be reachable |
| 2 | Weight | Higher |
| 3 | Local Preference | Higher |
| 4 | Originated Locally | Yes |
| 5 | AS Path | Shorter |
| 6 | Origin Code | IGP (i) |
| 7 | MED | Lower |
| 8 | Path Type | eBGP |
| 9 | IGP Metric | Lower |
| 10 | Age | Older |
| 11 | Router ID | Lower |
| 12 | Cluster List | Shorter |
| 13 | Neighbor IP | Lower |

---

## IV. Essential Interview & Exam Keywords

`BGP Decision Process`, `AS Path Prepend`, `Local Preference`,  
`Weight vs MED`, `iBGP vs eBGP`, `Well-Known Attributes`,  
`Optional Attributes`, `Route Map`, `RPL`, `Policy Control`,  
`Cisco IOS XE`, `Cisco IOS XR`.

---

**End of Notes – “BGP Path Selection Complete Technical Guide”**

## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)

