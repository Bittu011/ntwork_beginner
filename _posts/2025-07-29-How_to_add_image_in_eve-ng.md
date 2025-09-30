---
title: How to Upload the Image to EVE-NG
date: 2025-07-22 04:00:00 +0000
categories: [Networking_Topics]
tags: [eve-ng, networking, ciscolabs, virtualization, imageupload, homelab, networksimulation]
---


<h1 align="center">🚀 How to Upload an Image to EVE-NG (Ultimate Step-by-Step Guide)</h1>


Whether you're a network engineer, a student preparing for certifications, or an IT professional building labs, **EVE-NG (Emulated Virtual Environment - Next Generation)** is one of the best platforms out there.

> But one common challenge many users face is:

In this blog, we'll walk you through how to upload an image to EVE-NG, what tools you'll need, and provide best practices to ensure your lab runs smoothly.


<h2 align="center">📦 What Is an Image in EVE-NG?</h2>


An "image" refers to a virtual machine disk file (such as `.qcow2`, `.vmdk`, etc.) used to emulate network devices like Cisco routers, Palo Alto firewalls, Fortinet devices, and more.


<h2 align="center">✅ Prerequisites</h2>


Before uploading an image to EVE-NG, make sure you have the following ready:

<h3>📁 1. The Image File</h3>

-   Format: Typically `.qcow2` (for KVM), sometimes `.vmdk`, `.iso`, etc.
-   Source: Downloaded from vendor (e.g., Cisco, Palo Alto, Fortinet) or converted from another format.

<h3>🖥️ 2. EVE-NG Community or Pro Installed</h3>

-   Either in a VM (VMware/VirtualBox) or on a dedicated server.
-   Make sure EVE-NG is up and running.


<h3>🔧 3. Tools Required</h3>

Here are the main tools you’ll use to upload and configure images:


| Tool                    | Purpose                               | OS                      |
| ----------------------- | ------------------------------------- | ----------------------- |
| **WinSCP**              | Upload files via SCP                  | Windows                 |
| **FileZilla**           | Alternative SCP tool                  | Windows/Linux/Mac       |
| **Putty / Terminal**    | SSH into EVE-NG                       | Windows / macOS / Linux |
| **EVE-NG Convert Tool** | Convert `.vmdk` to `.qcow2` if needed | Optional                |
| **QEMU-img**            | Image conversion                      | Optional                |


<h2 align="center">🛠️ How to Upload an Image to EVE-NG (Step-by-Step)</h3>

Here’s a detailed guide to upload your image.

<h3>🔹 Step 1: Connect to EVE-NG Using WinSCP</h3>

1.  Download and install WinSCP
2.  Connect to your EVE-NG VM using:
    -   Host: Your EVE-NG IP
    -   Protocol: SCP
    -   Username: root
    -   Password: eve (default)

>   🔐 Change your default credentials for security.



<h3>🔹 Step 2: Navigate to the Right Directory</h3>

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


<h3>🔹 Step 3: Upload the Image File</h3>

-   Upload your `.qcow2` file to the folder you created.
-   Rename the file to match EVE-NG naming conventions:

```bash
virtioa.qcow2
```

>   📌 Some images need multiple files. Always check vendor documentation.


<h3>🔹 Step 4: Fix Permissions</h3>

After uploading, you must set the correct permissions. Use SSH (PuTTY or Terminal) to run:

```bash
/opt/unetlab/wrappers/unl_wrapper -a fixpermissions
```

This step is **critical**. Without it, your images might not boot properly.


<h3>🔹 Step 5: Refresh EVE-NG and Start Building</h3>


-   Go back to your EVE-NG web interface.
-   Create a new lab or open an existing one.
-   Add a new node → You should now see your image listed.

🎉 That’s it — your image is now ready to use!



<h2 align="center">💡 Pro Tips</h2>

-   🔁 Convert VMDK to QCOW2 using qemu-img:

```bash
qemu-img convert -f vmdk -O qcow2 input.vmdk output.qcow2
```

🗂️ Always check EVE-NG’s Image Naming Guide for specific device formats.
🌐 Test connectivity after uploading by pinging your image node or starting a simple topology.


<h2 align="center">🧰 Recommended Tools List</h2>

| Tool      | Description                 | Link                                                       |
| --------- | --------------------------- | ---------------------------------------------------------- |
| WinSCP    | Secure file transfer client | [Download](https://winscp.net/)                            |
| PuTTY     | SSH client for Windows      | [Download](https://www.putty.org/)                         |
| FileZilla | GUI SCP/SFTP client         | [Download](https://filezilla-project.org/)                 |
| qemu-img  | Image converter (CLI)       | [Docs](https://wiki.qemu.org/Documentation/Tools/qemu-img) |


<h2 align="center">🔚 Conclusion</h2>

Uploading an image to EVE-NG is straightforward once you understand the structure. By following the steps above, you can add any device you want — Cisco, Juniper, Palo Alto, Fortinet, and more — to create powerful network labs.



## 🙌 Connect With Me

[![GitHub](https://img.shields.io/badge/GitHub-Profile-black?style=for-the-badge&logo=github)](https://github.com/Ntwork-Beginner)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/ntworkbeginner/)
[![YouTube](https://img.shields.io/badge/YouTube-Subscribe-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@Ntwork_Beginner)
[![Gmail](https://img.shields.io/badge/Gmail-Mail-red?style=for-the-badge&logo=gmail)](mailto:your.bittudhillon011@gmail.com)
