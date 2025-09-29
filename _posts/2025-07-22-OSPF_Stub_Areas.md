---
title: OSPF Stub Areas
date: 2025-07-22 04:00:00 +0000
categories: [Networking_Topics]
tags: [ospf_stub_areas, ccna, ccnp, cisco, network_topics]
---

<h1 align="center">OSPF Stub Area Concepts – Comprehensive Technical Notes</h1>



<h2 align="center">1. Introduction</h2>

In large-scale OSPF (Open Shortest Path First) networks, **stub areas** are used to optimize scalability and reduce unnecessary routing overhead. They achieve this by filtering specific **LSA (Link-State Advertisement) types**, minimizing the **Link-State Database (LSDB)** size, and replacing external routes with a **default route**.


**Key Benefits**:

-   🚀 **Scalability & Performance** – Smaller LSDB → faster SPF calculations, reduced CPU/memory usage.
-   🔒 **LSA Filtering** – Blocks certain LSA types from entering the area.
-   🌐 **Default Route Injection** – ABR injects a 0.0.0.0/0 summary route to guide traffic outside the area.


<h2 align="center">2. OSPF Area Types and LSA Behavior</h2>


| **OSPF Area Type**            | **LSAs Allowed** | **LSAs Blocked** | **Default Route?**                                    |
| ----------------------------- | ---------------- | ---------------- | ----------------------------------------------------- |
| **Standard Stub Area (OSA)**  | Type 1, 2, 3     | Type 4, 5        | Yes (ABR generates via Type 3 LSA)                    |
| **Totally Stubby Area (TSA)** | Type 1, 2        | Type 3, 4, 5     | Yes (ABR generates via Type 3 LSA)                    |
| **Not-So-Stubby Area (NSSA)** | Type 1, 2, 3, 7  | Type 4, 5        | Optional (manual via `default-information-originate`) |
| **Totally NSSA (TNSSA)**      | Type 1, 2, 7     | Type 3, 4, 5     | Yes (ABR generates via Type 3 LSA)                    |


<h3>Key LSA Handling</h3>

-   **Type 4 (ASBR Summary)**: Blocked in stub areas; suppresses ASBR reachability info.
-   **Type 5 (External)**: Blocks redistributed external routes (e.g., from BGP/EIGRP).
-   **Type 7 (NSSA External)**: Used only inside NSSA/TNSSA for redistributed routes; ABR converts Type 7 → Type 5 for backbone.
-   **Default Route**: Always injected as Type 3 Summary LSA.


<h2 align="center">3. Configuration Commands</h2>

<h3>A. Standard Stub Area (OSA)</h3>

-   **All routers in area** : ``` area <area-id> stub ```

<h3>B. Totally Stubby Area (TSA)</h3>

-   **ABR only** : ```area <area-id> stub no-summary```

-   **Other routers** : ```area <area-id> stub```

<h3>C. Not-So-Stubby Area (NSSA)</h3>

-   **All routers in area** :  ```area <area-id> nssa```

-   **Optional default route on ABR** : ```area <area-id> nssa default-information-originate```


<h3>D. Totally NSSA (TNSSA)</h3>

-   **ABR only** : ```area <area-id> nssa no-summary```

-   **Other routers** : ```area <area-id> nssa```


<h2 align="center">4. Technical Requirements & Restrictions</h2>

1.  **Uniform Configuration**: All routers in an area must agree on stub type (checked via OSPF Hello options).

2.  **Backbone Restriction**: Area 0 cannot be a stub area.

3.  **ASBR Restriction**:

    -   ❌ Not allowed inside Stub/Totally Stubby areas (no Type 5 LSAs).
    -   ✅ Allowed inside NSSA/TNSSA (Type 7 LSAs supported).

4.  **Virtual Links**: Cannot be created across stub areas.


<h2 align="center">5. Verification & Troubleshooting</h2>

<h3>🔍 Verification Commands</h3>

-   **Check OSPF Process**:
    -   OSPFv2 → ` show ip ospf `
    -   OSPFv3 → ` show ipv6 ospf` or `show ospfv3 `

-   **Check Routes**:
    -   OSPFv2 → `show ip route ospf`
    -   OSPFv3 → `show ipv6 route ospf`


✅ Example (TSA Routing Table):

```bash
O*IA 0.0.0.0/0 [110/2] via 10.34.1.3, GigabitEthernet0/0
```

<h3>⚡ Troubleshooting Adjacencies</h3>

-   **Common Issue**: Mismatched stub type → adjacency failure.
-   **Debugging Commands**:
    -   `debug ip ospf hello` (OSPFv2)
    -   `debug ospfv3 hello` (OSPFv3)
-   **Why?** – Stub/Transit area bit mismatch in OSPF Hello packets.


<h2>6. Interview & Revision Key Points</h2>

-   Stub areas = scalability + reduced LSDB.
-   Totally stubby = most restrictive (only intra-area + default route).
-   NSSA = allows redistribution (Type 7 LSAs).
-   TNSSA = NSSA + no Type 3/4/5 LSAs.
-   Area 0 cannot be stub.
-   Virtual links not supported in stub areas.
-   Always check default route injection (Type 3).



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
