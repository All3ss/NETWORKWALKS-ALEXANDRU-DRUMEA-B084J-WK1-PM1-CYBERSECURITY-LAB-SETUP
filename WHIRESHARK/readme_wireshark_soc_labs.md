# 🔐 Wireshark Network Traffic Analysis | SOC Lab 

**Building an isolated virtual lab for penetration testing and ethical hacking practice**

![](https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Ver-Wireshark%20v4.2-0070C0?style=flat-square&labelColor=000000) ![](https://img.shields.io/badge/OS-Windows%2011%2025H2-E87500?style=flat-square&labelColor=000000&logo=windows&logoColor=white) ![](https://img.shields.io/badge/Skill-Packet%20Analysis-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Network-192.168.1.0%2F24-238F89?style=flat-square&labelColor=000000) ![](https://img.shields.io/badge/Traffic%20Analysis-C00000?style=flat-square&labelColor=000000&logo=wireshark&logoColor=white) ![](https://img.shields.io/badge/Skill-Network%20Security-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000) ![](https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=wireshark&logoColor=white) ![](https://img.shields.io/badge/Alexandru%20Drumea-C00000?style=flat-square)

---

## 📌 Project Overview

This project focuses on **network packet capture, protocol analysis, and threat detection using Wireshark** across multiple SOC security lab scenarios (Labs 1–8).

The objective is to capture and analyze real-time network traffic, inspect DNS resolutions, examine TLS 1.3 handshakes, follow TCP streams for unencrypted data leakage, identify port scans (SYN scans), brute-force login attempts, and reconstruct VoIP (SIP/RTP) audio calls.

---

## 🎯 Objectives

The main objectives of this Wireshark project series are to:

-   Capture and filter live network traffic across Wi-Fi and Ethernet interfaces.
-   Filter and inspect **DNS queries and responses** (`dns.qry.name`) to resolve target domain IPs.
-   Analyze **TLS/HTTPS handshakes** (`tls.handshake.type == 1` for Client Hello, `tls.handshake.type == 2` for Server Hello) and extract Server Name Indication (SNI) extensions.
-   Reconstruct TCP conversation streams (**Follow TCP Stream**) to uncover unencrypted credentials, chat logs, and confidential files.
-   Detect **reconnaissance and port scanning attacks** (SYN flood/scan analysis).
-   Detect and analyze **Brute-Force authentication attacks** over HTTP.
-   Reconstruct and replay **VoIP calls (SIP/RTP)** to extract sensitive call metadata and unencrypted audio data.
-   Document network analysis findings and security countermeasures.

---

## 🛡️ Purpose of the Labs

The Wireshark labs provide hands-on experience in SOC packet analysis and network forensics:

-   **DNS Protocol Inspection:** Verifying domain query resolution and detecting malicious DNS requests.
-   **SSL/TLS Encryption Analysis:** Inspecting Client/Server Hello parameters, cipher suites, SNI values, and TLS 1.3 protocol negotiation.
-   **Plaintext Data Recovery:** Finding sensitive user credentials, Wi-Fi passwords, and confidential chat logs sent over HTTP/TCP streams.
-   **Incident Response & Attack Detection:** Identifying attacker IPs, target IPs, open/closed ports, and brute-force failure/success status codes.
-   **VoIP Telephony Forensics:** Inspecting SIP call setup, caller extensions, audio codecs, and replaying RTP streams.

---

## 🏗️ Lab Architecture & Network Topology

```text
[ Attacker / Client VM ] <---> [ Wireless / NAT Network Switch ] <---> [ Target / Web / DNS / VoIP Server ]
(192.168.1.29 / 192.168.0.105)         (10.0.0.0/24 or 192.168.1.0/24)            (79.79.79.79 / 192.232.216.135)
```

---

## ⚙️ Lab Environment Configuration

| 🧩 Component | ⚙️ Configuration / Value |
| --- | --- |
| 🖥️ Host OS | Windows 11 (25H2) |
| 🧰 Packet Analyzer | Wireshark v4.2.x |
| 📡 Network Interface | `\Device\NPF_{8521E575-1EF4-4A4E-946E-26CFA66B7B6B}` (Wi-Fi) |
| 🐉 Attacker / Client IP | `192.168.1.29` / `192.168.0.105` |
| 🌐 DNS Resolver | `79.79.79.79` / `8.8.8.8` |
| 🎯 Target IP | `192.232.216.135` (`networkwalks.com`) |
| 🔐 Protocols Analyzed | DNS, TCP, TLSv1.3, HTTP, SIP, RTP |

---

# 🪜 Wireshark Labs Analysis & Execution

## Lab 1. DNS Resolution & TLS 1.3 Handshake Inspection (`networkwalks.com`)

### Step 1. Filter DNS Query and Server IP

-   **Filter:** `dns.qry.name == "networkwalks.com"`
-   **Results:**
    -   Client IP: `192.168.1.29`
    -   DNS Server IP: `79.79.79.79` (Packets 3323, 3324)

### Step 2. Domain Resolution

-   **Resolved IP:** `192.232.216.135` (Packet 3325, Type A Record)

### Step 3. TLS Client Hello & SNI Extension

-   **Filter:** `tls.handshake.type == 1`
-   **Result:** Packet 3347 initiated TLS 1.3 Client Hello with SNI `networkwalks.com` to `192.232.216.135` on destination port `443` (TCP segment length: 524 bytes).

### Step 4. TLS Server Hello Response

-   **Filter:** `tls.handshake.type == 2`
-   **Result:** Packet 3367 received Server Hello response from `192.232.216.135`, negotiating **TLS 1.3**.

---

## Lab 2. DNS & TLS Analysis (`microsoft.com`)

### Step 1. DNS Query

-   **Filter:** `dns.qry.name == "microsoft.com"`
-   **Client IP:** `192.168.0.105` | **DNS Server:** `8.8.8.8` (Packet 26)

### Step 2. DNS Response & Resolved IP Range

-   **Resolved IP Addresses:** `20.70.246.20`, `20.76.201.171`, `20.112.250.133`, `20.231.239.246`, `20.236.44.162` (Packet 27)

### Step 3. TLS Handshake & Transport Layer Details

-   **TLS Client Hello (Packet 33):** Destination IP `20.70.246.20`, SNI `microsoft.com`, Port `52431` -> `443`, Packet Length: 215 bytes.
-   **TLS Server Hello (Packet 35):** Response from `20.70.246.20` negotiating TLS 1.3.

---

## Lab 3. Reconnaissance & Port Scan Detection (SYN Scan)

-   **Objective:** Identify the attacker's IP and target ports.
-   **Filter:** `tcp.flags.syn == 1 && tcp.flags.ack == 0`
-   **Key Findings:**
    -   Identified Attacker IP and Target IP.
    -   Detected probed ports, open ports (SYN-ACK responses), and closed ports (RST responses).
    -   Score: **100/100 marks (100%)**.

---

## Lab 4. TCP Stream Following (Unencrypted Chat & Credentials)

-   **Objective:** Reconstruct TCP conversation streams to recover cleartext communications.
-   **Method:** Right-click TCP packet -> **Follow -> TCP Stream**.
-   **Key Findings:**
    -   Recovered chat port, conversation initiator, stream index, and chat transcript length.
    -   Uncovered sensitive temporary passwords, drive mappings, and Guest Wi-Fi keys.
    -   Score: **100/100 marks (100%)**.

---

## Lab 5. TCP Stream Following - Part 2 (Web Data & Hidden Directories)

-   **Objective:** Extract HTTP GET requests, status codes, and downloaded files.
-   **Key Findings:**
    -   Located hidden web pages and directories.
    -   Identified backup server IPs, download directions, HTTP status codes (200 OK / 404 Not Found), and author details of captured reports.
    -   Score: **100/100 marks (100%)**.

---

## Lab 6. HTTP Plaintext Credential Extraction

-   **Objective:** Detect cleartext authentication over unencrypted HTTP.
-   **Filter:** `http.request.method == "POST"`
-   **Key Findings:**
    -   Identified login page URL, target system IP, total login attempts.
    -   Extracted cleartext usernames and passwords directly from packet payload lines.
    -   Score: **100/100 marks (100%)**.

---

## Lab 7. Brute-Force Attack Analysis

-   **Objective:** Detect automated login brute-forcing.
-   **Key Findings:**
    -   Identified protocol, target IP, attacker IP, and targeted username.
    -   Determined HTTP failure status codes (e.g., 401/403) vs. success code (200 OK).
    -   Found total failed login attempts and the exact password that successfully authenticated the attacker.
    -   Score: **100/100 marks (100%)**.

---

## Lab 8. VoIP Call Replay & Telephony Forensics

-   **Objective:** Reconstruct SIP signaling and RTP audio streams.
-   **Tools:** Wireshark Telephony -> VoIP Calls -> Play Streams.
-   **Key Findings:**
    -   Identified Call Setup Protocol (SIP) and Voice Protocol (RTP).
    -   Recovered caller extension, caller name, call receiver, and audio codec (e.g., G.711 / G.722).
    -   Extracted sensitive spoken credit card numbers and expiry dates directly from audio replay.
    -   Score: **100/100 marks (100%)**.

---

# 🔎 Summary of Completed SOC Labs

| 🧪 Lab Number | 🎯 Topic / Title | 📊 Result | 🏆 Score |
| --- | --- | --- | --- |
| **Lab 1** | DNS & TLS Handshake (`networkwalks.com`) | Completed | 100% |
| **Lab 2** | DNS & TLS Handshake (`microsoft.com`) | Completed | 100% |
| **Lab 3** | Identify the Attacker's IP (SYN Scan) | Outstanding | 100/100 |
| **Lab 4** | Follow TCP Stream (Unencrypted Chat) | Outstanding | 100/100 |
| **Lab 5** | Follow TCP Stream, Part 2 (HTTP/Web) | Outstanding | 100/100 |
| **Lab 6** | HTTP Plain Text Credentials | Outstanding | 100/100 |
| **Lab 7** | Brute Force Catch | Outstanding | 100/100 |
| **Lab 8** | VoIP Call Replay | Outstanding | 100/100 |

---

# 💡 Key Learnings & SOC Insights

### 1\. DNS & TLS SNI Analysis

DNS queries disclose target domain names prior to encrypted connection setups. The TLS Client Hello SNI extension reveals the target hostname even when the payload is encrypted with TLS 1.3.

### 2\. Risks of Plaintext Protocols (HTTP/TCP)

Unencrypted HTTP POST requests and raw TCP streams allow attackers or analysts to perform packet inspection and extract sensitive passwords, chat logs, and confidential files without needing decryption keys.

### 3\. Port Scanning & Reconnaissance Detection

Excessive SYN packets without completing the 3-way handshake indicate active port scanning. Differentiating between SYN-ACK (open) and RST (closed) packets allows analysts to identify mapped host attack surfaces.

### 4\. Brute-Force Attack Signatures

High volumes of HTTP POST requests originating from a single IP address with high rates of authentication failure status codes followed by a single success status code signal a successful brute-force attack.

### 5\. VoIP Eavesdropping

Unencrypted SIP signaling and RTP streams allow unauthorized audio capture and call replay, exposing confidential spoken data such as PII and credit card numbers. Enforcement of SRTP (Secure RTP) and TLS for SIP is required.

---

# 🔐 Security & Ethical Use

This repository and its documentation are intended strictly for educational purposes and authorized SOC packet analysis training. All packet captures were gathered in an isolated laboratory environment.

---

# 🔗 Tools & Resources

-   **Wireshark:** [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)
-   **NetworkWalks Cybersecurity:** [https://www.networkwalks.com](https://www.networkwalks.com)

---

# 👤 Author

**Alexandru Drumea**  
Computing Undergraduate  
Cyber Intern

## 🔗 [LinkedIn Profile](https://www.linkedin.com/in/alex-drumea-7971a8155)

## 📌 Project Information

**Program Name:** Cybersecurity at Networkwalks   
**Labs:** 1 – 8   
**Project:** Wireshark Packet Analysis & SOC   
**Repository:** GitHub