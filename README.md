# Vulnerability Assessment & Penetration Testing (VAPT) Report

![License](https://img.shields.io/badge/License-Academic--Internal-blue.svg)
![Target](https://img.shields.io/badge/Target-Metasploitable2-red.svg)
![Attacker OS](https://img.shields.io/badge/OS-Kali%20Linux-dragon.svg)
![Severity](https://img.shields.io/badge/Max%20Severity-Critical%20(CVSS%2010.0)-brightred.svg)

## 📌 Executive Summary
This repository contains the full technical report and methodology for a **Vulnerability Assessment and Penetration Testing (VAPT)** project conducted against a **Metasploitable2** target machine in an isolated virtual lab environment. 

The assessment successfully identified and exploited a critical backdoor vulnerability in the `vsftpd 2.3.4` service, leading to **unauthenticated remote root access** (complete system compromise). Additional security analysis was performed using **Nessus Essentials** and **Burp Suite Community Edition**.

---

## 🎯 Scope & Lab Architecture

- **Author:** Swastik Chowdhury *(B.Tech CSE, University of Kalyani)*
- **Target Machine:** Metasploitable2 VM (`192.168.1.3`)
- **Attacker Machine:** Kali Linux VM (`192.168.1.4`)
- **Hypervisor:** Oracle VM VirtualBox
- **Network Mode:** Isolated Host-Only Virtual Network (No WAN/LAN exposure)
- **Primary Document:** [`VAPT_Report .pdf`](./VAPT_Report%20.pdf)

---

## 🛠️ Tools & Frameworks Used

| Category | Tool / Framework | Usage |
| :--- | :--- | :--- |
| **Network Scanning** | Nmap (`7.9x`) | Service version detection & port enumeration |
| **Exploitation** | Metasploit Framework (`msfconsole`) | Executing `vsftpd_234_backdoor` exploit |
| **Vulnerability Assessment** | Nessus Essentials | Automated network vulnerability scanning |
| **Web Application Security** | Burp Suite Community Edition | Intercepting proxy & SQL Injection payload testing |

---

## 🔍 Key Findings & Vulnerability Matrix

| Port / Service | Service Version | Severity | Finding / Impact | CVE |
| :--- | :--- | :--- | :--- | :--- |
| **21/TCP (FTP)** | vsftpd 2.3.4 | **Critical** (CVSS 10.0) | Malicious backdoor allowing unauthenticated root shell via Metasploit. | [CVE-2011-2523](https://nvd.nist.gov/vuln/detail/CVE-2011-2523) |
| **21/TCP (FTP)** | Anonymous FTP | **Medium** | Anonymous access allowed without credentials. | - |
| **22/TCP (SSH)** | OpenSSH 4.7p1 | **Low / Info** | Outdated SSH service version with known legacy issues. | - |
| **53/UDP (DNS)** | BIND / DNS | **Medium** (CVSS 5.3) | DNS Server Cache Snooping Remote Info Disclosure (via Nessus scan). | - |
| **80/TCP (HTTP)** | DVWA Web App | **High** | SQL Injection (`id=1' OR '1'='1`) in DVWA endpoint tested via Burp Repeater. | - |

---

## 🚀 Penetration Testing Execution Highlights

### 1. Nmap Reconnaissance
```bash
nmap -sV -sC -p- 192.168.1.3
