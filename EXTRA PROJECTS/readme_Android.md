# 🔐 Android Security Lab Setup | Mobile Security

**Building an isolated virtual lab for penetration testing and ethical hacking practice**

![](https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Ver-Virtualbox%20v7.x-0070C0?style=flat-square&labelColor=000000) ![](https://img.shields.io/badge/Guest%20OS-Android%209-E87500?style=flat-square&labelColor=000000&logo=android&logoColor=white) ![](https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000) ![](https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000) ![](https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white) ![](https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000)

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Android 9 / Linux targets.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

---

## 🎯 Objectives

The main objectives of this project are to:

-   Install and configure VirtualBox on Windows 11 host.
-   Import and configure the virtual machine using the `android-x86_64-9.0-r2.iso` image (`Android9-Lab`).
-   Create a private **NAT Network** for the cybersecurity lab.
-   Configure network connectivity and IP assignment.
-   Verify network connectivity and performance.
-   Resolve VirtualBox NEM (Snail execution mode) and service errors.
-   Document the complete setup process.
-   Prepare the environment for future cybersecurity projects.

---

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for activities such as:

-   Network reconnaissance
-   Port scanning
-   Vulnerability assessment
-   Packet analysis
-   Web and mobile security testing
-   Exploitation practice
-   Security-tool experimentation

⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

## 🏗️ Lab Architecture

![](http://localhost:56497/StaticNbiktCnb/externalProject/1-screenshot-title-image.png)

Additional target machines can be added to the same virtual network in future projects.

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
| --- | --- |
| 🖥️ Host OS | Windows 11 (Build 26200) |
| 🧠 Host RAM | 16 GB (15.8 GB usable) |
| ⚡ Processor | AMD Ryzen 7 7435HS (8 Cores / 16 Threads) |
| 💻 Host Model | ASUS TUF Gaming A15 FA507NVR |
| 🧰 Hypervisor | VirtualBox 7.x |
| 🐉 Security / Guest OS | Linux 64-bit (Android 9 - `Android9-Lab`) |
| 💿 ISO Image File | `android-x86_64-9.0-r2.iso` |
| 🧠 VM RAM | 2048 MB (2 GB) |
| ⚡ VM vCPUs | 1 vCPU |
| 💾 Disk Path | `D:\NetworkWalksAcademy\VM Android\Android9-Lab\Android9-Lab.vdi` |
| 🌐 Virtual Network | NAT Network |
| 📡 Network Address | 10.0.0.0/24 |
| 🚪 Default Gateway | 10.0.0.1 |
| 🌍 DNS Server | 8.8.8.8 |

---

# 🪜 Lab Setup Procedure

## Step 1. Install 7-Zip & VirtualBox

7-Zip and VirtualBox were installed on the Windows 11 host system (ASUS TUF Gaming A15).

---

## Step 2. Create the NAT Network

A dedicated NAT Network was created in VirtualBox.

Configuration:

-   Network Name: NatNetwork
-   IPv4 Prefix: 10.0.0.0/24
-   DHCP: Enabled
-   IPv6: Disabled

![](http://localhost:56497/StaticNbiktCnb/externalProject/2-screenshot-network-settings-1.png)

---

## Step 3. Configure Android 9 Virtual Machine

The virtual machine `Android9-Lab` was created and configured in VirtualBox using the `android-x86_64-9.0-r2.iso` image mounted as an optical storage drive to perform the system installation.

Virtual Hard Disk location:

-   `D:\NetworkWalksAcademy\VM Android\Android9-Lab\Android9-Lab.vdi`

Initial resources allocated:

-   RAM: 2048 MB
-   CPU: 1 vCPU
-   Optical Drive: Attached `android-x86_64-9.0-r2.iso`

![](http://localhost:56497/StaticNbiktCnb/externalProject/3-screenshot-kali-linux.png)

---

## Step 4. Configure Virtual Network Settings

The virtual machine network adapter was assigned to the NAT Network for isolation and controlled routing.

![](http://localhost:56497/StaticNbiktCnb/externalProject/4-screenshot-kali-network-settings.png)

---

# 🔎 Lab Verification

| ✅ Test | 🧾 Command / Action | 🎯 Expected Result |
| --- | --- | --- |
| 🌐 Check IP address | `ip a` | Valid IP within 10.0.0.0/24 |
| 📡 Test gateway | `ping 10.0.0.1` | Successful replies |
| 🌍 Test Internet connectivity | `ping 8.8.8.8` | Successful replies |
| 🧰 Check Execution Engine | VirtualBox VBox.log inspection | Native HW Virtualization active |

---

# 🐞 Problems Encountered & Solutions

## Problem 1. VirtualBox "Snail Execution Mode" (NEM Engine Active)

**Symptom:** In the VBox log file, the following message appeared: `NEM: NEMR3Init: Snail execution mode is active!` and `CPUM: No hardware-virtualization capability detected`.

**Cause:** Windows 11 features such as **Core Isolation / Memory Integrity** or Hyper-V components were blocking direct access to AMD-V hardware virtualization on the AMD Ryzen 7 7435HS processor.

**Solution:**

1.  Open **Windows Security** $\\rightarrow$ **Device Security** $\\rightarrow$ **Core Isolation details**.
2.  Turn off **Memory Integrity**.
3.  Open Command Prompt as Administrator and run:
    
    ```cmd
    bcdedit /set hypervisorlaunchtype off
    ```
    
4.  Restart the host machine.

---

## Problem 2. HGCM Service Error (`VERR_HGCM_SERVICE_NOT_FOUND`)

**Symptom:** Log showed `ERROR [COM]: ... VERR_HGCM_SERVICE_NOT_FOUND` during guest properties initialization.

**Solution:** Reinstalled/updated VirtualBox Guest Additions and verified guest service compatibility.

---

# 💡 What I Learned

Through this project, I learned how to create and configure a virtual environment for cybersecurity practice.

Key takeaways:

1.  **Hardware Virtualization Setup:** Managing Windows 11 Hyper-V and Core Isolation settings to enable hardware-accelerated AMD-V execution in VirtualBox.
2.  **Resource Allocation:** Understanding how vCPU and RAM allocation impact virtual machine responsiveness.
3.  **NAT Network Isolation:** Setting up isolated virtual networks to connect test systems safely.

---

# 🔐 Security & Ethical Use

This laboratory is intended strictly for educational purposes only.

---

# 👤 Author

**Alexandru Drumea**  
Computing Undergraduate  
Cybersecurity Intern

## 🔗 [LinkedIn Profile](https://www.linkedin.com/in/alex-drumea-7971a8155)

## 📌 Project Information

**Program Name:** Cybersecurity at Networkwalks   
**Project:** Cybersecurity Lab Setup   
**Repository:**  GitHub