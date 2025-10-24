---
title: BGP Attributes
date: 2025-12-02 04:00:00 +0000
categories: [BGP]
tags: [bgp, ibgp, ebgp, ntworkengineer, bgp_attrubutes]
---

# ⚡ BGP Path Attributes — Quick Revision Notes

> 📘 **Goal:** Fast-track learning & interview-ready reference for the 5 most important BGP attributes.

---

## 🧱 1. AS_PATH (Type Code: 2)

| Property | Summary |
|:----------|:----------|
| **Classification** | Well-known Mandatory |
| **Purpose** | Lists all AS numbers the route has passed through → used for loop prevention & best path |
| **Structure** | Ordered `AS_SEQUENCE`, unordered `AS_SET` (used in aggregation) |
| **Best Path Rule** | Prefer **shortest AS_PATH** |
| **Manipulation** | `set as-path prepend <asn>` → makes path longer, less preferred (controls **inbound** traffic) |
| **Transitivity** | ✅ Yes (carried in UPDATEs) |
| **Notes** | Loop prevention + Inbound TE tool |

---

## 🌉 2. NEXT_HOP (Type Code: 3)

| Property | Summary |
|:----------|:----------|
| **Classification** | Well-known Mandatory |
| **Purpose** | Identifies the next router IP to reach the destination prefix |
| **Behavior** | eBGP → next-hop = neighbor IP<br>iBGP → next-hop unchanged |
| **Manipulation** | `neighbor <ip> next-hop-self` → make local router next-hop |
| **Requirement** | Must be **reachable** in local routing table |
| **Transitivity** | ✅ Yes |
| **Notes** | Ensures correct forwarding — key for iBGP reachability |

---

## 🌐 3. ORIGIN (Type Code: 1)

| Property | Summary |
|:----------|:----------|
| **Classification** | Well-known Mandatory |
| **Purpose** | Shows **how** route entered BGP (source of origination) |
| **Codes** | `i` (IGP – network statement)<br>`e` (EGP – old protocol)<br>`?` (Incomplete – redistribution) |
| **Preference Order** | IGP (i) > EGP (e) > Incomplete (?) |
| **Manipulation** | `set origin igp` / `set origin incomplete` |
| **Transitivity** | ✅ Yes |
| **Notes** | Affects internal preference; used in best-path Step #7 |

---

## ⚖️ 4. WEIGHT (Cisco Proprietary)

| Property | Summary |
|:----------|:----------|
| **Classification** | Optional Non-Transitive (local only) |
| **Purpose** | Determines **local outbound route preference** (first best-path step) |
| **Range** | 0–65,535 → higher = more preferred |
| **Default** | 32,768 (local) / 0 (learned) |
| **Manipulation** | `set weight <value>` or `neighbor <ip> weight <value>` |
| **Transitivity** | ❌ No |
| **Notes** | Local router only → controls local exit traffic |

---

## 🏛️ 5. LOCAL_PREFERENCE (LOCAL_PREF) (Type Code: 5)

| Property | Summary |
|:----------|:----------|
| **Classification** | Well-known Discretionary |
| **Purpose** | Defines **AS-wide outbound preference** (second best-path step) |
| **Default** | 100 |
| **Rule** | Higher LOCAL_PREF = more preferred exit |
| **Manipulation** | `set local-preference <value>` or `neighbor <ip> local-preference <value>` |
| **Propagation** | ✅ iBGP only (not sent to eBGP) |
| **Notes** | Controls outbound routing for the entire AS |

---

## 🧠 BGP Best Path Order (Top 5 Steps)

| Step | Attribute | Rule |
|:------|:-------------|:----------------|
| 1️⃣ | **Weight** | Higher = better |
| 2️⃣ | **Local Preference** | Higher = better |
| 3️⃣ | **Locally originated** | Preferred over learned |
| 4️⃣ | **AS_PATH** | Shorter = better |
| 5️⃣ | **Origin** | IGP > EGP > Incomplete |

---

## 🧾 Quick Summary

| Attribute | Influence | Scope | Transitive | Typical Use |
|:------------|:-------------|:-------------|:-------------|:-------------|
| ⚖️ **Weight** | Outbound | Local Router | ❌ No | Local path control |
| 🏛️ **Local Pref** | Outbound | Entire AS | ✅ Internal only | AS exit preference |
| 🧱 **AS_PATH** | Inbound | Global | ✅ Yes | Loop prevention, inbound TE |
| 🌐 **Origin** | Internal | Global | ✅ Yes | Route source & preference |
| 🌉 **Next Hop** | Forwarding | Global | ✅ Yes | Ensures reachability |

---

> ⚙️ **Tip:**  
> - **Weight & Local Pref → Outbound control**  
> - **AS_PATH → Inbound control**  
> - **NEXT_HOP → Reachability**  
> - **ORIGIN → Preference hint**

---

🧩 **Perfect For:**  
CCNA/CCNP/ENARSI Learners | Network Engineers | Interview Quick Revision  



---
## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
