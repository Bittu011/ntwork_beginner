---
title: EIGRP LoadBalancing
date: 2025-03-28 05:00:00 +0000
categories: [Networking_Topics]
tags: [networking, eigrp, networkengineer, cisco, itlab]
---

# 📚 EIGRP Load Balancing

EIGRP (Enhanced Interior Gateway Routing Protocol) is a smart routing protocol that can send traffic across multiple paths to the same destination. This helps make your network faster, more reliable, and efficient.

EIGRP supports two types of load balancing:

- **Equal-Cost Load Balancing**
- **Unequal-Cost Load Balancing** (this is what makes EIGRP special!)

---

## 🔁 1. Equal-Cost Load Balancing (ECMP)

When all available paths to a destination have the **SAME cost/metric**.

### ✅ Key Points:
- EIGRP will use multiple paths only if they have the **exact same metric**.
- It splits the traffic evenly across these paths.
- By default, up to **4 paths** can be used.
- You can increase this using the `maximum-paths` command (up to **16 paths**).

### 🧠 Think of it like:
Having 4 identical roads from your house to school. You send 1/4 of the cars down each road.

---

## 🌀 2. Unequal-Cost Load Balancing

When paths to the same destination have **different metrics**, but are still usable.

This is what makes EIGRP powerful — it can use slower or less preferred paths if they are still safe (loop-free).

### 🚨 But there’s a rule! (Very important)

To avoid loops, the path must be a **Feasible Successor**.

### 🔐 What’s a Feasible Successor?

It’s a backup route that is:
- Loop-free
- Not the best, but still valid

### ✅ It meets the Feasibility Condition:

The reported distance from the neighbor must be **less than your current best distance** to that destination.

---

## ⚙️ Using variance for Unequal-Cost Load Balancing

The `variance` command tells EIGRP:

> “I’m okay with using paths that are up to **X times worse** than the best path.”

- `variance` is a multiplier (1–128)
- Default is 1 (only equal-cost paths allowed)

### Example:

- Best path = metric 100
- `variance 2` → accept paths up to metric 200

---

## 📦 How Traffic is Shared (Traffic Sharing)

- **With equal-cost paths:** traffic is shared **evenly**.
- **With unequal-cost paths:** traffic is shared **proportionally based on metrics**.

### Example:

| Path | Metric | Traffic Share Ratio |
|-------|--------|---------------------|
| A (best) | 100    | 2 packets            |
| B (backup) | 200    | 1 packet             |

EIGRP will send 2 packets through A for every 1 packet through B (2:1 ratio).

---

## 🧰 Configuration Summary

| Feature         | Command (Classic Mode)         | Description                          |
|-----------------|-------------------------------|------------------------------------|
| Max Paths       | `maximum-paths <1-16>`         | Max number of paths to install     |
| Variance        | `variance <1-128>`             | Allows unequal-cost paths           |
| Traffic Share   | `traffic-share balanced` (default) | Share traffic by metric ratio  |
|                 | `traffic-share min`            | Use only best path for traffic      |

---

## 👨‍🏫 Easy Analogy

You're delivering pizza to a town.

- **Equal-Cost Load Balancing:**  
  You have 4 highways of equal speed. You send your delivery trucks evenly across all.

- **Unequal-Cost Load Balancing:**  
  You have 1 fast highway and a couple of slower but still good roads. If the highway gets crowded, you send some trucks through the slower roads — but only if you're sure they won’t get lost (**Feasibility Condition**).

---

## 🧠 Final Tips

- Remember the **Feasibility Condition**. It’s the safety rule for avoiding loops.
- Use **variance** only when you want to include backup paths with higher metrics.
- Always set **maximum-paths** high enough to include all the paths you want EIGRP to use.
- Use `show ip eigrp topology` to see:
  - Feasible Successors
  - Reported and Feasible Distances
  - Active vs Passive routes


## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
