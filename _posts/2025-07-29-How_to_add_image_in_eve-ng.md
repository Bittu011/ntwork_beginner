---
title: How to Upload the Image to EVE-NG
date: 2025-07-29 04:00:00 +0000
categories: [Networking_Topics]
tags: [eve-ng, networking, ciscolabs, virtualization, imageupload, homelab, networksimulation]
---

# 🚀 How to Upload an Image to EVE-NG (Ultimate Step-by-Step Guide)

Whether you're a network engineer, a student preparing for certifications, or an IT professional building labs, EVE-NG (Emulated Virtual Environment - Next Generation) is one of the best platforms out there.

> But one common challenge many users face is:

In this blog, we'll walk through how to upload an image to EVE-NG, the tools needed, and best practices to ensure the lab runs smoothly.

## 📦 What Is an Image in EVE-NG?

An "image" refers to a virtual machine disk file (such as `.qcow2`, `.vmdk`, etc.) used to emulate network devices like Cisco routers, Palo Alto firewalls, Fortinet devices, and more.

## ✅ Prerequisites

Before uploading an image to EVE-NG, make sure the following are ready:

### 📁 1. The Image File

- Format: Typically `.qcow2` (for KVM), sometimes `.vmdk`, `.iso`, etc.
- Source: Downloaded from vendor (e.g., Cisco, Palo Alto, Fortinet) or converted from another format.

### 🖥️ 2. EVE-NG Community or Pro Installed

- Either in a VM (VMware/VirtualBox) or on a dedicated server.
- Make sure EVE-NG is up and running.

### 🔧 3. Tools Required

Here are the main tools used to upload and configure images:

| Tool | Purpose | OS |
| --- | --- | --- |
| WinSCP | Upload files via SCP | Windows |
| FileZilla | Alternative SCP tool | Windows/Linux/Mac |
| Putty / Terminal | SSH into EVE-NG | Windows / macOS / Linux |
| EVE-NG Convert Tool | Convert `.vmdk` to `.qcow2` if needed | Optional |
| QEMU-img | Image conversion | Optional |

## 🛠️ How to Upload an Image to EVE-NG (Step-by-Step)

Here’s a detailed guide to upload the image.

### 🔹 Step 1: Connect to EVE-NG Using WinSCP

1. Download and install WinSCP
2. Connect to the EVE-NG VM using:
   - Host: Your EVE-NG IP
   - Protocol: SCP
   - Username: root
   - Password: eve (default)

> 🔐 Change the default credentials for security.

### 🔹 Step 2: Navigate to the Right Directory

EVE-NG stores images in specific directories. Navigate to:

```bash
/opt/unetlab/addons/qemu/
```


Each image must go in a folder named after the device type, like:

```bash
/opt/unetlab/addons/qemu/cisco-asav-921/
```


📁 The folder must follow the correct naming convention, usually like:


```bash
vendor-platform-version
e.g., cisco-asav-921
```


### 🔹 Step 3: Upload the Image File

- Upload the `.qcow2` file to the folder created.
- Rename the file to match EVE-NG naming conventions:

```bash
virtioa.qcow2
```



> 📌 Some images need multiple files. Always check vendor documentation.

### 🔹 Step 4: Fix Permissions

After uploading, set the correct permissions. Use SSH (PuTTY or Terminal) to run:


```bash
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```


This step is critical. Without it, images might not boot properly.

### 🔹 Step 5: Refresh EVE-NG and Start Building

- Go back to the EVE-NG web interface.
- Create a new lab or open an existing one.
- Add a new node → The image should now be listed.

🎉 That’s it — the image is now ready to use!

## 💡 Pro Tips

- Convert VMDK to QCOW2 using qemu-img:

```bash
qemu-img convert -f vmdk -O qcow2 input.vmdk output.qcow2
```


- Always check EVE-NG’s Image Naming Guide for specific device formats.
- Test connectivity after uploading by pinging the image node or starting a simple topology.

## 🧰 Recommended Tools List

| Tool | Description | Link |
| --- | --- | --- |
| WinSCP | Secure file transfer client | https://winscp.net/ |
| PuTTY | SSH client for Windows | https://www.putty.org/ |
| FileZilla | GUI SCP/SFTP client | https://filezilla-project.org/ |
| qemu-img | Image converter (CLI) | https://wiki.qemu.org/Documentation/Tools/qemu-img |

## 🔚 Conclusion

Uploading an image to EVE-NG is straightforward once the structure is understood. By following the steps above, it’s possible to add any device—Cisco, Juniper, Palo Alto, Fortinet, and more—to build powerful network labs.

## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
