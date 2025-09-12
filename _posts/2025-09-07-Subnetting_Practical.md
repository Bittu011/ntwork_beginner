---
title: Subnetting Practical
date: 2025-09-07 07:00:00 +0000
categories: [Networking_Practical]
tags: [subnetting, networking, ccna, ccnp]
---

# 🧠 Subnetting Practical (Handwritten Style)

A complete step-by-step guide to solving subnetting questions — just the way interviews and exams expect.

Every type of problem is solved in detail so you can **revise fast and effectively**.

---

## 🔹 Q1. How Many Subnets and Hosts Can Be Created?

**Example**: `192.168.1.0/26`

**Step-by-Step**  
1. Host bits = `32 - 26 = 6`  
2. Usable hosts = `2^6 - 2 = 62`  
3. Borrowed bits = `/26 - /24 = 2` → Subnets = `2^2 = 4`

✅ **Answer**: `4 subnets`, `62 usable hosts per subnet`

---

## 🔹 Q2. Find Subnet Mask and Ranges for Prefix

**Example**: `/27`

**Steps**  
1. Subnet mask = `255.255.255.224`  
2. Block size = `256 - 224 = 32`  
3. Ranges = `0–31`, `32–63`, `64–95`, ...

✅ **Answer**: Subnets increase by 32, each with 30 usable hosts

---

## 🔹 Q3. Which Subnet Does an IP Belong To?

**Example**: IP = `192.168.1.70` | Network = `192.168.1.0/27`

**Steps**  
1. Block size = 32  
2. Ranges = `0–31`, `32–63`, `64–95`  
3. 70 lies in `64–95`

✅ **Answer**: `192.168.1.64/27`

---

## 🔹 Q4. How Many Bits to Borrow?

**Example**: `10.0.0.0/24 → Need 16 subnets`

**Steps**  
1. `2^x ≥ 16` → `x = 4`  
2. New prefix = `/24 + 4 = /28`  
3. Usable hosts = `2^4 - 2 = 14`

✅ **Answer**: Borrow `4 bits` → `/28` → `16 subnets`, `14 usable hosts` each

---

## 🔹 Q5. Smallest Subnet for Given Hosts

**Example**: Need `50 hosts`, starting from `172.16.0.0/24`

**Steps**  
1. `2^n - 2 ≥ 50` → `n = 6`  
2. Host bits = `6` → Prefix = `32 - 6 = /26`  
3. Subnet mask = `255.255.255.192`

✅ **Answer**: Use `/26` (62 usable hosts)

---

## 🔹 Q6. VLSM Allocation

**Given**: `192.168.10.0/24` → For 100, 50, 25, and 10 hosts

**Steps**  
1. 100 → `/25` → Range: `192.168.10.0 – 192.168.10.127`  
2. 50 → `/26` → Range: `192.168.10.128 – 192.168.10.191`  
3. 25 → `/27` → Range: `192.168.10.192 – 192.168.10.223`  
4. 10 → `/28` → Range: `192.168.10.224 – 192.168.10.239`

✅ **Answer**: Efficient VLSM allocation without overlaps

---

## 🔹 Q7. Point-to-Point Links

- `/30` → 4 total, 2 usable  
  👉 Example: `10.1.1.0/30` → Hosts: `10.1.1.1`, `10.1.1.2`  
- `/31` → 2 total, 2 usable (RFC 3021)  
  👉 Example: `10.1.1.4/31` → Hosts: `10.1.1.4`, `10.1.1.5`

✅ **Answer**: Use `/31` to save IPs on point-to-point links

---

## 🔹 Q8. Find Broadcast Address

**Example**: `192.168.5.96/27`

**Steps**  
1. Block size = `32`  
2. Subnet range = Starts at 96 → Next = 128  
3. Broadcast = `128 - 1 = 127`

✅ **Answer**: Broadcast = `192.168.5.127`

---

## 🔹 Q9. Supernetting (Combining Networks)

Supernetting = combining smaller networks into a larger one  
🔑 Rule: Networks must be **consecutive** and **aligned properly**

### Example 1:  
Combine `192.168.0.0/24` + `192.168.1.0/24`  
→ New prefix = `/23`  
→ Range = `192.168.0.0 – 192.168.1.255`

✅ **Answer**: Supernet = `192.168.0.0/23`

### Example 2:  
Combine `10.0.0.0/24` + `10.0.1.0/24`  
→ New prefix = `/23`  
→ Range = `10.0.0.0 – 10.0.1.255`

✅ **Answer**: Supernet = `10.0.0.0/23`

### Example 3:  
Combine:  
- `172.16.0.0/24`  
- `172.16.1.0/24`  
- `172.16.2.0/24`  
- `172.16.3.0/24`  

→ 4 networks = `2^2 = 4` → Borrow 2 bits → `/22`  
→ Range = `172.16.0.0 – 172.16.3.255`

✅ **Answer**: Supernet = `172.16.0.0/22`

---

## 🔹 Q10. Trick Cases to Know

- `/32` → Single IP only (loopback), no hosts  
- `/31` → 2 usable hosts, no broadcast (RFC 3021)  
- `/30` → Classic point-to-point with 2 usable hosts

---

# 📝 Practice Problems (With Solutions)

### 1. `192.168.2.0/24 → Create 8 Subnets`

✅  
- Need 8 subnets → `2^3 = 8` → Borrow 3 bits → `/27`  
- Block size = `256 - 224 = 32`  
- Ranges: `0–31`, `32–63`, ..., `224–255`  
- Each subnet = `30 usable hosts`

✅ **Answer**: 8 subnets of `/27`

---

### 2. Broadcast of `10.0.4.0/22`

✅  
- Mask = `255.255.252.0`  
- Block size = 4 (in 3rd octet)  
- Next = `10.0.8.0` → Broadcast = `10.0.7.255`

✅ **Answer**: `10.0.7.255`

---

### 3. Which Subnet for `172.16.35.200/20`?

✅  
- Mask = `255.255.240.0` → Block size = 16 (3rd octet)  
- Ranges: `0–15`, `16–31`, `32–47`, ...  
- 35 → in range `32–47`

✅ **Answer**: `172.16.32.0/20`

---

### 4. Aggregate `192.168.8.0/24` + `192.168.9.0/24`

✅  
- Two consecutive `/24` networks  
- Combine → `/23`  
- Range = `192.168.8.0 – 192.168.9.255`

✅ **Answer**: `192.168.8.0/23`

---

### 5. Need 500 Hosts → Which Prefix?

✅  
- `2^n - 2 ≥ 500` → `n = 9`  
- Host bits = 9 → Prefix = `/23`  
- Mask = `255.255.254.0` → 510 usable

✅ **Answer**: Use `/23` (510 usable hosts)

---


### 📌 Essential Subnetting Formulas

```bash
Hosts per Subnet = 2^h – 2 (where h = host bits)
Number of Subnets = 2^n (where n = borrowed bits)
Block Size = 256 – Subnet Mask Value
New Prefix = Old Prefix + Borrowed Bits
Subnet Mask = Prefix → Decimal Mask
Broadcast Address = Last IP in Subnet
Network Address = First IP in Subnet
Supernetting Rule = Combine only contiguous networks with common prefix
```

**Special Cases:**

- /32 = single host (loopback)

- /31 = 2 usable (RFC 3021, point-to-point)

- /30 = 2 usable (classic point-to-point)