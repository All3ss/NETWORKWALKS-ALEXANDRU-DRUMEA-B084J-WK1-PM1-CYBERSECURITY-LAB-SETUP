# NETWORKWALKS-ALEXANDRU-DRUMEA-B084J-WK1-PM1-CYBERSECURITY-LAB-SETUP KALI LINUX
NetworkWalks Cybersecurity Internship 2026 – Week 1, Project 1. Hands-on experience with virtualisation, networking and cybersecurity fundamentals.

**Building an isolated virtual lab for penetration testing and ethical hacking practice**

![](https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Ver-Virtualbox%20v7.2.22-0070C0?style=flat-square&labelColor=000000) ![](https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white) ![](https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000) ![](https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white) ![](https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white) ![](https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white) ![](https://img.shields.io/badge/Author-Alexandru%20Drumea-C00000?style=flat-square)

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Kali Linux.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

---

## 🎯 Objectives

The main objectives of this project are to:

-   Install and configure VirtualBox.
-   Install/import Kali Linux as a virtual machine.
-   Create a private **NAT Network** for the cybersecurity lab.
-   Configure network connectivity for Kali Linux.
-   Assign a consistent IP address to the Kali VM.
-   Verify network connectivity and DNS resolution.
-   Take a clean VM snapshot for recovery.
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
-   Web security testing
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
| 🖥️ Host OS | Windows 11 Home (Build 26200) |
| 💻 Host Model | ASUS TUF Gaming A15 FA507NVR |
| 🧠 Host RAM | 16 GB |
| ⚡ Processor | AMD Ryzen 7 7435HS with Radeon Graphics |
| 🧰 Hypervisor | VirtualBox 7.2.22 r175245 |
| 🐉 Security OS | Kali Linux 2026.2 (Kernel 6.19.14+kali-amd64) |
| 🔲 Kali Processors | 2 vCPU |
| 🧠 Kali RAM | 2048 MB |
| 🌐 Virtual Network | NAT Network |
| 📡 Network Address | 10.0.0.0/24 |
| 🐧 Kali IP Address | 10.0.0.2/24 |
| 🚪 Default Gateway | 10.0.0.1 |
| 🌍 DNS Server | 8.8.8.8 |
| 🔮 Future VM Range | 10.0.0.3–10.0.0.99 |

---

# 🪜 Lab Setup Procedure

## Step 1. Install 7-Zip

7-Zip was installed to extract the Kali Linux virtual-machine package (`kali-linux-2026.2-virtualbox-amd64.7z`).

**Tool:** 7-Zip

---

## Step 2. Install VirtualBox

VirtualBox (v7.2.22) was installed as the hypervisor on Windows 11.

---

## Step 3. Create the NAT Network

A dedicated NAT Network was created in VirtualBox.

Configuration:

```text
Network Name: NatNetwork
IPv4 Prefix:  10.0.0.0/24
DHCP:         Enabled
IPv6:         Disabled
```

![](http://localhost:56497/StaticNbiktCnb/externalProject/2-screenshot-network-settings-1.png)

A **NAT Network** was selected because multiple virtual machines connected to the same NAT Network can communicate with one another while also having outbound network connectivity.

This will allow future attacker and target VMs to communicate within the lab.

---

## Step 4. Import Kali Linux

The Kali Linux virtual machine was imported into VirtualBox on the ASUS TUF Gaming host.

The VM network adapter was configured as follows:

```text
Adapter 1
Attached to: NAT Network
Network:     NatNetwork
Adapter Type: Intel PRO/1000 MT Desktop
```

The VM was allocated:

```text
Processors: 2 vCPUs
RAM: 2048 MB
```

![](http://localhost:56497/StaticNbiktCnb/externalProject/3-screenshot-kali-linux.png)

A shared folder was also configured for transferring required files between the Windows 11 host operating system and the Kali VM.

---

## Step 5. Configure the Kali Linux Network

The Kali Linux network configuration was checked and configured with a consistent IPv4 address.

Example configuration:

```text
IP Address: 10.0.0.2
Subnet Mask: 255.255.255.0
Gateway: 10.0.0.1
DNS: 8.8.8.8
```

A consistent IP address makes it easier to document the lab and reference the Kali machine in future exercises.

![](http://localhost:56497/StaticNbiktCnb/externalProject/4-screenshot-kali-network-settings.png)

---

## Step 6. Create a Clean VM Snapshot

After completing the initial configuration, a VirtualBox snapshot was created.

Example snapshot name:

```text
Clean Kali - Network Setup
```

The snapshot represents the clean baseline of the laboratory.

If a future exercise changes or damages the VM configuration, the machine can be restored to this baseline.

---

# 🔎 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result |
| --- | --- | --- |
| 🌐 Check IP address | `ip a` | Correct Kali IP displayed |
| 📡 Test gateway | `ping 10.0.0.1` | Successful replies |
| 🌍 Test Internet connectivity | `ping 8.8.8.8` | Successful replies |
| 🔎 Test DNS resolution | `nslookup google.com` | Domain resolves |
| 🧰 Verify Nmap | `nmap --version` | Nmap version displayed |
| 🔄 Verify snapshot | Restore snapshot and run `ip a` | Baseline configuration restored |

### Example Results

```text
IP Address:
10.0.0.2/24

Gateway:
10.0.0.1

DNS:
8.8.8.8
```

---

# 🐞 Problems Encountered & Solutions

Documenting problems is an important part of the project.

## Problem 1. Internet Connectivity After Static IP Configuration

After manually configuring the IPv4 settings, Internet connectivity may fail depending on the Kali/NetworkManager configuration.

One workaround used during this lab was:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

The network connection was then restarted and connectivity was tested again.

---

## Problem 2. Hardware Virtualization / Hyper-V Engine Mode (NEM)

The VM log indicated execution in NEM (Native Execution Manager) mode because Windows 11 Hyper-V features were active on the AMD Ryzen 7 host system.

The issue was managed by:

1.  Ensuring AMD SVM (Secure Virtual Machine) / Hardware Virtualization was enabled in the ASUS BIOS.
2.  Configuring VirtualBox to interface properly with Hyper-V APIs on Windows 11.
3.  Allocating 2 vCPUs and 2048 MB RAM to maintain stable performance within the VM.

---

# 💡 What I Learned

Through this project, I learned how to create and configure a virtual environment for cybersecurity practice.

The most important concepts I learned include:

### 1\. NAT vs NAT Network

A standard NAT configuration and a NAT Network serve different purposes.

A NAT Network allows multiple VMs connected to the same virtual network to communicate with one another while providing network address translation for external connectivity.

### 2\. Virtual Machine Networking

I learned how VirtualBox virtual network adapters connect virtual machines to different types of networks and how network configuration affects communication between machines.

### 3\. Static IP Configuration

I learned how to configure and verify IPv4 addressing, subnet masks, gateways, and DNS settings in Kali Linux.

### 4\. VM Snapshots

I learned that a clean snapshot should be created **before performing risky or experimental activities**.

This provides a known-good recovery point for future cybersecurity exercises.

### 5\. Documentation

I learned that documenting commands, configuration, screenshots, problems, and solutions is an important part of a professional cybersecurity project.

---

# 🔐 Security & Ethical Use

This laboratory is intended strictly for educational purposes only.

---

# 🔗 Tools & Resources

-   **7-Zip:** [https://7-zip.org/download.html](https://7-zip.org/download.html)
-   **VirtualBox:** [https://virtualbox.org/wiki/Downloads](https://virtualbox.org/wiki/Downloads)
-   **Kali Linux:** [https://kali.org/get-kali](https://kali.org/get-kali)

---

# 👤 Author

**Alexandru Drumea**  
Computing Undergraduate  
Cybersecurity Intern

🔗 [LinkedIn Profile](https://www.linkedin.com/in/alex-drumea-7971a8155)
---

## 📌 Project Information

**Project:** Cybersecurity & Pentesting Lab Setup   
**Platform:** VirtualBox & Kali Linux  
**Repository:** GitHub
