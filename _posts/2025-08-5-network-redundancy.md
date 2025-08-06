---
title: Network Redundancy + VLAN Security Lab
date: 2025-08-5 11:11:00 +0000
categories: [Networking_Labs]
tags: [hsrp, vlan, security, cisco, cml, nat, dhcp, acl, etherchannel, inter-vlan, networklab]
---

# 🚀 Network Redundancy + VLAN Security Lab

<div style="text-align: center; margin: 15px 0;">
  <a href="https://www.youtube.com/channel/your-channel" target="_blank" style="margin: 0 10px;">
    <img src="https://cdn-icons-png.flaticon.com/48/1384/1384060.png" alt="YouTube" width="32" height="32" style="vertical-align: middle;">
  </a>
  <a href="https://www.linkedin.com/in/your-profile" target="_blank" style="margin: 0 10px;">
    <img src="https://cdn-icons-png.flaticon.com/48/174/174857.png" alt="LinkedIn" width="32" height="32" style="vertical-align: middle;">
  </a>
  <a href="https://github.com/your-github" target="_blank" style="margin: 0 10px;">
    <img src="https://cdn-icons-png.flaticon.com/48/733/733553.png" alt="GitHub" width="32" height="32" style="vertical-align: middle;">
  </a>
</div>

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
