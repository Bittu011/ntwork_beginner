---
title: Zabbix AutoMap
date: 2026-03-10 04:00:00 +0000
categories: [Zabbix]
tags: [Zabbix, CCNA, Automation, AutoMap]
---

# Zabbix Auto Map Script – Automatically Generate Network Maps in Zabbix

Creating network topology maps manually in Zabbix can take a lot of time, especially in larger environments.  
With the **Zabbix AutoMap Script**, you can automatically generate maps based on host tags and relationships.

In this guide, I will show you **step-by-step how to automatically generate a Zabbix map using a Python script.**

---


You can also follow the complete walkthrough in the video below.

---

<!-- Embed full YouTube video -->
<iframe width="100%" height="400"
  src="https://www.youtube.com/embed/-8vMzBxttec"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen>
</iframe>

---

# Step 1 – Create a Network Topology

First, create a **network topology**.

You can do this in:

- **EVE-NG**
- **GNS3**
- **Your Production Network**

For this example, we are using:

- 1 Router (R1)
- 2 Switches
- 1 Linux machine running Zabbix

---

# Step 2 – Configure SNMP on Router

On the **router**, configure SNMP so Zabbix can monitor the device.

Example configuration:

```bash
snmp-server community zabbix_ro RO
snmp-server location LAB
snmp-server contact admin

snmp-server enable traps
snmp-server host 192.168.X.X version 2c zabbix_ro
```

-   This allows the Zabbix server to collect SNMP data from the router.

---
# Step 3 – Configure SNMP on Switches

Apply the **same SNMP configuration on switches** so Zabbix can monitor them.


Example:
```bash
snmp-server community zabbix_ro RO
snmp-server location LAB
snmp-server contact admin

snmp-server enable traps
snmp-server host 192.168.X.X version 2c zabbix_ro
```

---

# Step 4 – Configure Network on Linux

Now configure the **Linux machine** where you will access Zabbix.

Assign an **IP address** and **default gateway**.

```bash
ip addr add 192.168.10.10/24 dev ens3

ip route add default via 192.168.10.1

sudo nano /etc/resolve.conf     => we have to change IP (8.8.8.8) in nameserver
```

-   After this, verify connectivity to the network.

---


# Step 5 – Install  Zabbix Server

You can install **Zabbix Server** in different ways:

-   Install on **Linux / Ubuntu**
-   Use a **prebuilt appliance**

---

## Default Credentials

### System Login

```bash
Username: root
Password: zabbix
```

### Zabbix Frontend Login

```bash
Username: Admin
Password: zabbix
```

## ⚙️ Configure Zabbix Network Settings

Like any Linux server, configure the following network settings:

- 🌐 **IP Address**
- 🚪 **Default Gateway**
- 🧭 **DNS Server**

> ✅ Once the network is configured, you can access the **Zabbix Web GUI** from your browser.

Example: 192.168.X.X


---

# Step 6 — Configure Zabbix

## 6.1 Create Hosts

Inside the **Zabbix Web Interface**:

1. Navigate to  
   **Configuration → Hosts**

2. Click  
   **Create Host**

3. Configure the following parameters:

| Setting | Description |
|------|------|
| **Hostname** | Name of the device or server |
| **Host Group** | Logical group for the host |
| **Templates** | Monitoring templates to apply |


💡 **Tip:**  
Assigning templates automatically adds predefined monitoring items, triggers, and graphs.

---

### Example Host Setup

```text
Hostname: S1
Host Group: automap, Network Devices
Template: Cisco IOS by SNMP
```

## 6.2 Create a Blank Map

To create a network map in Zabbix:

1. Navigate to : Monitoring → Maps


2. Click **Create Map**.
3. Create a **new empty map**.

####  Example Map Name   : automap


💡 **Tip:**  
Maps allow you to visually represent your infrastructure and monitor the status of hosts, links, and services.

---

## 6.3 Create a Host Group

Create a **host group** that will be used for the automap.

### Steps

1. Navigate to:
Configuration → Host groups


2. Click **Create host group**.
3. Enter the group name.

#### Example : automap


4. Save the configuration.

✔ Now assign your **devices/hosts** to this host group so they can be automatically included in the map.


## 6.4 Configure Host Tags (Important)

The **automap script** uses **host tags** to understand device relationships in your network.

Inside the host configuration : Host → Tags


Add the required tags depending on the device type.

---

## Router Example

Configure the following tags for a router:

```text
Tag: am.host.type
Value: Router

Tag: am.link.connect_to
Value: R1
```

## Switch Example

Since switches connect to Router R1, configure the tag below:

```bash
Tag: am.link.connect_to
Value: R1
```

💡 This tells the automap script that the switch is connected to Router R1, allowing it to correctly build the network topology.


## Additional Optional Tags

These control how the map looks.

```bash
am.link.color  808080
am.link.draw_type  1
am.link.label
```

## 6.5 Create an API Token

The script needs a **Zabbix API Token**.

Steps:

-  Go to User Settings
-   Open API Tokens
-   Click Create Token
-   Copy the generated token

We will use this **token when running the script**.

---

# Step 7 – Prepare the Zabbix Server

In this example, the Zabbix server is running on Rocky Linux.

## 7.1 Install Python

```bash
sudo dnf install python3.9 -y
```

## 7.2 Install Required Tools

```bash
sudo yum install -y git
sudo yum install -y nano
```

## 7.3 Clone the AutoMap Script

Clone the repository from GitHub.

```bash
git clone https://github.com/SomoneIT/zabbix-AutoMapper.git
```

Move into the directory:

```bash
cd zabbix-AutoMapper
```

## 7.4 Fix Script Compatibility

Open the script:

```bash
nano automap.py
```

At the last lines, change double commas ("") to single commas ("")

Example:

```bash
logger.debug(automaper.graph.graph.vs['x'])
logger.debug(automaper.graph.graph.vs['y'])
```

## 7.5 Create Python Virtual Environment

Create and activate a virtual environment.

```bash
python3.9 -m venv venv
```

Activate it:

```bash
source venv/bin/activate
```

Upgrade pip:

```bash
python3.9 -m pip install --upgrade pip
```

Install dependencies:

```bash
python3.9 -m pip install -r requirements.txt
```

## 7.6 Run the Automap Script

Now execute the script:

```bash
python3.9 automap.py --zabbix_host 192.168.X.X --zabbix_folder / --zabbix_port 80 --zabbix_scheme http --map automap --group automap --map_layout tree --log_level debug --verbose --token your-token-paste-here
```

Replace:

```bash
192.168.X.X
```

with your **Zabbix server IP**.

Also paste your **API token**.


---

# Step 8 – Map Will Be Created Automatically

Once the script runs successfully:

-   Zabbix will read host tags
-   Identify device relationships
-   Automatically generate a **network topology map**

You can view it under:

```bash
Monitoring → Maps
```

Your automatically generated topology map will appear there.




# 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)