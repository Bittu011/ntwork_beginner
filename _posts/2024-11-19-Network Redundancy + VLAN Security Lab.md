---
title: Network Redundancy + VLAN Security Lab
date: 2024-11-19 05:00:00 +0000
categories: [Networking_Labs]
tags: [hsrp, vlan, security, cisco, cml, nat, dhcp, acl, etherchannel, inter-vlan, networklab]
---

# 🚀 Network Redundancy + VLAN Security Lab

## 🖼️ See the Network Design

Curious what the lab looks like?  
**Check out the full network topology diagram here:**

<!-- Embed full YouTube video -->
<iframe width="100%" height="400"
  src="https://www.youtube.com/embed/sIpxjIj7xF4"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen>
</iframe>

As someone just starting out in the networking field, I'm excited to share that I've completed one of my most advanced hands-on labs using Cisco CML — and it’s working exactly as intended! 👏 This project demonstrates real-world skills with HSRP, VLANs, EtherChannel, NAT, ACLs, and more.

---

### 📌 Project Highlights

- 🧩 Set up HSRP with 2 ISPs (Airtel & BSNL) for gateway redundancy  
- 🎛️ Created 4 VLANs across two Layer 2 switches, connected with EtherChannel  
- 🔄 Configured DHCP on routers to dynamically assign IPs to VLANs  
- 🛡️ Applied NAT with ACLs — allowing only VLAN 10 & 20 to access the internet  
- 🚫 VLAN 30 & 40 are successfully blocked via ACL — verified through NAT debug and access-list logs  
- ✔️ Verified internet access using ping 8.8.8.8 and checked NAT translations with `show ip nat translations`  

---

### 🛠️ Tools & Learning

- Cisco CML  
- Alpine Linux (PCs)  
- NAT, ACL, DHCP, HSRP, Inter-VLAN Routing  
- Commands like `debug ip nat`, `show access-lists`, `show ip dhcp binding`, etc.  
- Still a beginner, not started CCNP yet — learning every day through labs like this!

---

### 🎥 Soon, I’ll be uploading:

- GitHub repo with the full config files (coming soon)  
- YouTube walkthrough with setup and explanation (video in progress)  

---

### 🙏 Feedback & Connect

If you notice any mistakes in my upcoming GitHub repo or YouTube video — I’d truly appreciate your feedback. It will help me grow and improve as I continue my journey toward becoming a Network Engineer. 🤝  
Let’s connect if you’re also learning or love working with labs!

#BeginnerNetworkEngineer #CiscoCML #HSRP #NAT #ACL #VLAN #DHCP #EtherChannel #CCNA #NetworkingLabs #GitHub #YouTube #LearningInPublic #LinkedInLearningJourney


## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
