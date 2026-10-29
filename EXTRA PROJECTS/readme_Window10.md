# 🔐 Windows 10 Security Lab Setup | Endpoint Security

**Building an isolated virtual lab for penetration testing and resolving virtualization conflicts**

![](https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Ver-VirtualBox%20v7.2.22-0070C0?style=flat-square&labelColor=000000) ![](https://img.shields.io/badge/Host%20OS-Windows%2011-0078D4?style=flat-square&labelColor=000000&logo=windows&logoColor=white) ![](https://img.shields.io/badge/CPU-AMD%20Ryzen-ED1C24?style=flat-square&labelColor=000000&logo=amd&logoColor=white) ![](https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white) ![](https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Alexandru%20Drumea-C00000?style=flat-square)

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox, analyzing runtime performance logs, and diagnosing virtualization conflicts on Windows 11 host systems.

The purpose of the lab is to create a controlled environment where virtual operating systems (such as Windows Server/Client or Linux security distributions) can be deployed, configured, and tested safely. During setup, host-level security features and BIOS virtualization technologies were analyzed to ensure optimal hardware-assisted execution.

---

## 🎯 Objectives

The main objectives of this project are to:

-   Install and configure VirtualBox (v7.2.22).
-   Mount and deploy virtual machine images (e.g., `Windows.iso`).
-   Analyze VirtualBox log files (`VBox.log`) to identify execution bottlenecks.
-   Detect and resolve conflicts between VirtualBox and Windows 11 **Core Isolation (Memory Integrity)**.
-   Ensure proper hardware-assisted virtualization (**AMD-V / SVM Mode**) in host BIOS.
-   Disable Hyper-V conflicts preventing full native execution.
-   Install necessary VirtualBox Extension Packs for USB and device integration.
-   Document the complete diagnostic and resolution process.

---

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning, network configuration, and authorized testing.

It can be used for activities such as:

-   OS deployment and network configuration
-   Analyzing hypervisor and host hardware interactions
-   Vulnerability assessment and security testing
-   Experimenting with VirtualBox virtualization drivers (NEM vs. HM)

⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test.

---

## 🏗️ Lab Architecture

```
+-----------------------------------------------------------------------+
|                            HOST SYSTEM                                |
|  ASUS TUF Gaming A15 | AMD Ryzen CPU | Windows 11 Home (x64)           |
+-----------------------------------------------------------------------+
                                   |
              +--------------------+--------------------+
              | Hypervisor level (VirtualBox v7.2.22)   |
              +--------------------+--------------------+
                                   |
                        +----------+----------+
                        |  Virtual Machine    |
                        |  Windows.iso        |
                        +---------------------+
```

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
| --- | --- |
| 🖥️ Host OS | Windows 11 Home (64-bit) |
| 💻 Host System | ASUS TUF Gaming A15 FA506IHRB\_FA506IHRB |
| ⚡ Host CPU | AMD Ryzen 5 4600H with Radeon Graphics |
| 🧠 Host RAM | 15.3 GB RAM |
| 🧰 Hypervisor | Oracle VM VirtualBox 7.2.22 |
| 📀 Mounted Media | `D:\NetworkWalksAcademy\Windows.iso` |
| 🧩 Extension Pack | None (Recommended to install v7.2.22) |
| ⚙️ Execution Mode | NEM (Native Execution Manager) -> Snail |

---

# 🪜 Lab Setup Procedure

## Step 1. VirtualBox Installation & ISO Mounting

VirtualBox version 7.2.22 was installed on the host system (`Windows 11 Home`). The target operating system media was configured using a virtual optical drive attached to the controller:

-   **Source Path:** `D:\NetworkWalksAcademy\Windows.iso`

---

## Step 2. Initial VM Launch & Log Inspection

Upon launching the virtual machine, performance degradation was observed. Inspecting `VBox.log` revealed that hardware virtualization (AMD-V) was not being directly utilized by VirtualBox, forcing it into fallback execution mode:

```text
NEM: NEMR3Init: Snail execution mode is active!
HM: HMR3Init: Attempting fall back to NEM: AMD-V is not available
```

---

## Step 3. Root Cause Analysis via Log Parsing

Analyzing the log file showed two major factors preventing hardware-assisted virtualization:

1.  **Core Isolation (Memory Integrity):** Active in Windows 11.
    
    ```text
    Core Isolation (Memory Integrity): ENABLED
    ```
    
2.  **AMD-V / SVM Status:** Virtualization extensions blocked or disabled in BIOS.

---

## Step 4. Remediation Steps for Host Virtualization

To restore full hardware virtualization speed and exit "Snail Mode", the following host adjustments were identified and applied:

### A. Disable Core Isolation in Windows 11

1.  Open **Windows Security**.
2.  Navigate to **Device Security** -> **Core isolation details**.
3.  Set **Memory integrity** to **Off**.
4.  Restart the host machine.

### B. Enable AMD-V (SVM Mode) in UEFI/BIOS

1.  Reboot the ASUS TUF Gaming A15 laptop and press **F2** / **DEL** to enter BIOS.
2.  Press **F7** to open **Advanced Mode**.
3.  Locate **SVM Mode** (AMD Virtualization) and set it to **Enabled**.
4.  Save changes and exit (**F10**).

### C. Disable Hyper-V Features

Run Command Prompt as Administrator and execute:

```cmd
bcdedit /set hypervisorlaunchtype off
```

Restart the system to apply changes.

---

# 🔎 Lab Verification

| ✅ Test | 🧾 Method / Log Target | 🎯 Expected Result |
| --- | --- | --- |
| 📀 ISO Mount | VirtualBox Storage Settings | `Windows.iso` attached |
| ⚡ Execution Mode Check | Inspect `VBox.log` | `HM` (Hardware Mode) active |
| 🐢 Snail Mode Elimination | Search `NEMR3Init` in log | No "Snail mode is active" error |
| 🛡️ Extension Pack Check | VirtualBox Extension Manager | Extension Pack installed |

---

# 🐞 Problems Encountered & Solutions

## Problem 1. VirtualBox Running in "Snail Execution Mode" (NEM)

**Description:** The VM was extremely slow and log files logged `NEMR3Init: Snail execution mode is active!`.

**Cause:** Windows 11 Core Isolation / Memory Integrity was enabled, locking hardware virtualization features away from third-party hypervisors like VirtualBox.

**Solution:**

1.  Disabled **Memory Integrity** under Windows Security settings.
2.  Disabled hypervisor auto-launch via `bcdedit /set hypervisorlaunchtype off`.
3.  Rebooted system.

---

## Problem 2. AMD-V Virtualization Not Available

**Description:** `HMR3Init: Attempting fall back to NEM: AMD-V is not available` appeared in `VBox.log`.

**Cause:** AMD SVM Mode was disabled in the laptop BIOS firmware.

**Solution:**

1.  Entered BIOS via **F2** on startup.
2.  Enabled **SVM Mode** under Advanced CPU settings.
3.  Saved configuration and booted Windows 11.

---

# 💡 What I Learned

Through analyzing this log and setting up the lab, I learned key hypervisor and host system diagnostic skills:

1.  **Log Analysis:** How to read VirtualBox diagnostic logs (`VBox.log`) to identify performance bottlenecks.
2.  **Virtualization Drivers:** The difference between **HM** (Hardware-assisted Virtualization) and **NEM** (Native Execution Manager / Snail Mode).
3.  **OS-Level Conflicts:** How security mechanisms in modern operating systems (such as Windows 11 Memory Integrity) interact with hypervisors.
4.  **BIOS Configurations:** The role of AMD-V / SVM Mode in hardware-level virtual machine execution.

---

# 🔗 Tools & Resources

-   **VirtualBox Downloads:** [https://virtualbox.org/wiki/Downloads](https://virtualbox.org/wiki/Downloads)
-   **VirtualBox Extension Packs:** [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

---

# 👤 Author

**Alexandru Drumea**  
Computing Undergraduate  
Cybersecurity Intern

🔗 [LinkedIn Profile](https://www.linkedin.com/in/alex-drumea-7971a8155)
---

## 📌 Project Information

**Program Name:** Cybersecurity at Networkwalks   
**Week:** 01  
**Project:** VirtualBox Log Analysis & Lab   
**Repository:** GitHub