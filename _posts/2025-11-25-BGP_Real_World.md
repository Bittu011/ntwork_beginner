---
title: BGP - The Real-World Guide (Not Boring)
date: 2025-11-25 04:00:00 +0000
categories: [BGP]
tags: [bgp, ibgp, ebgp, ntworkengineer, sd&d]
---

<h1 align="center"> 📡 BGP - The Real-World Guide (Not Boring)</h1>

```
BGP (Border Gateway Protocol) is how routers across 
different networks (Autonomous Systems - AS) talk 
to each other to exchange route information.
```



## 🌐 BGP Types - eBGP vs iBGP

| Type  | Description                       | Preferred When                                               |
|-------|-----------------------------------|--------------------------------------------------------------|
| eBGP  | Between DIFFERENT ASes            | Connecting to ISP, cloud, external networks                  |
| iBGP  | Inside the SAME AS                | Sharing BGP routes within your own routers or data centers  |



## 🤝 BGP Neighbours (Peers)


BGP needs neighbours to share routing info.
You can use physical IPs or loopbacks.

✅ Use loopbacks + "update-source" for stability
✅ Use "ebgp-multihop" if not directly connected

```bash

## 🔁 eBGP - External BGP

✅ Preferred when:
- Connecting to another AS (e.g. ISP, cloud provider)
- Advertising your public IPs
- Receiving external internet routes

🧠 Notes:
- Default TTL = 1 (direct connect)
- Use `ebgp-multihop` if not directly connected
- You CAN use `network` command

---

## 🔄 iBGP - Internal BGP

✅ Preferred when:
- Sharing routes between routers in the same AS
- Data center or enterprise BGP backbone
- Redistributing eBGP-learned routes internally

🧠 Notes:
- iBGP needs full-mesh or Route Reflectors
- Next-hop is NOT changed by default → IGP must reach it
- You CAN use `network` command here too

---

## 🔃 iBGP Full-Mesh vs Route Reflector

| Design          | Preferred When                                      |
|------------------|----------------------------------------------------|
| Full-Mesh        | Small networks (3–5 routers)                       |
| Route Reflector  | Larger networks (5+ routers), ISPs, DC cores       |

🧠 RR allows non-full mesh by letting one router reflect routes to others.

---

## 🌍 Loopback vs Physical Interface

| Interface Used   | Preferred When                                          |
|------------------|--------------------------------------------------------|
| Loopback         | You want stable BGP peering over multiple paths        |
| Physical         | Simpler setups with direct cables                      |

Use: neighbor X.X.X.X update-source loopback0

```


## 🔢 BGP `network` Command

✅ You can use `network` in both **eBGP** and **iBGP**

🧠 How it works:
- Tells BGP: "If this route exists in my routing table, advertise it"
- Must match exact prefix
- Good for manual control

### ✅ Preferred When:
- You want to control what routes are advertised
- Advertising static or IGP-known prefixes

---

## 🔄 Redistribute into BGP

✅ You CAN redistribute static or IGP routes into BGP  
⚠️ **Use with caution** — not preferred without filtering

### ❌ Not Preferred When:
- You want stability and clean route control
- You don't have route-maps or filters

### ✅ Preferred When:
- You have many dynamic routes to advertise
- You're okay managing filters (prefix-lists, route-maps)

---

## 🧰 Route Filtering Tools

| Tool             | Purpose                                     |
|------------------|---------------------------------------------|
| Prefix-List      | Allow/block specific IP prefixes            |
| Route-Map        | Modify BGP attributes (LP, MED, tags, etc)  |
| Distribute-List  | Basic filtering (rarely used with BGP)      |
| Communities      | Tag routes for easier policy control        |

---

## ⚖️ Controlling Traffic – BGP Attributes

| Attribute         | Used For                                     | Preferred When                                          |
|------------------|----------------------------------------------|---------------------------------------------------------|
| Local Preference | Outbound traffic (higher = preferred)        | Choose best exit from your AS                          |
| AS Path          | Inbound traffic (shorter = preferred)        | Influence how others come to you                      |
| MED              | Suggest best inbound route (lower = better)  | You have multiple links to same neighbour             |
| Weight (Cisco)   | Local to the router only                     | Tie-breaker, not shared with others                   |

---

## 📦 Use Cases – What’s Preferred?

| Use Case                         | Preferred Setup                          |
|----------------------------------|------------------------------------------|
| Advertise public prefix          | eBGP with `network`                      |
| Share loopbacks internally       | iBGP with `network`                      |
| Connect to 2 ISPs                | Dual eBGP + AS path prepending           |
| Share eBGP routes internally     | iBGP with route reflector or full-mesh   |
| Prefer one ISP for outbound      | Set higher Local Preference              |
| De-prioritize inbound route      | AS Path Prepending                       |
| Remote peering (not directly connected) | Use loopback + ebgp-multihop        |

---

## 🚨 What’s NOT Preferred

❌ iBGP without full mesh or route reflector  
❌ Redistribute BGP into IGP (can cause loops)  
❌ Advertising too many routes without filters  
❌ Not ensuring next-hop reachability in iBGP  

---

## 🧯 BGP vs IGP – When to Use Each?

| Protocol | Preferred For                                 |
|----------|-----------------------------------------------|
| OSPF / EIGRP / IS-IS (IGP) | Internal routing (fast convergence) |
| BGP      | External routing, policy control, WAN scale   |

```
Use IGP for underlay (reachability), 
Use BGP for overlay (control, flexibility)
```

---

## ✅ Quick Recap Table

| Goal                                | Preferred Method                       |
|-------------------------------------|----------------------------------------|
| Connect to ISP                      | eBGP                                   |
| Advertise internal services         | iBGP + network statement               |
| Internal reachability               | IGP (OSPF, EIGRP, etc.)                |
| Multi-router internal setup         | iBGP with Route Reflector              |
| Stable peering                      | Loopbacks + update-source              |
| Load balancing out                  | Local Preference                       |
| Load balancing in                   | AS Path Prepending or MED              |
| Avoid iBGP full mesh                | Route Reflector                        |
| Avoid route leaks                   | Prefix-lists + route-maps              |

---

```
🔥 Final Tip: 
Always filter what you advertise. 
The Internet doesn't like surprises.
```

---
## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
