# Week 1 - Red Teaming Lab

## Overview
This repository contains the documentation and results of a Red Teaming lab conducted on a Metasploitable2 virtual machine in a controlled environment. The objective was to simulate real-world attack scenarios and identify security weaknesses.

---

## Objectives
- Perform reconnaissance and scanning
- Identify vulnerabilities in the target system
- Exploit identified weaknesses
- Demonstrate post-exploitation techniques
- Understand real-world attack methodologies

---

## Tools Used
- Nmap
- Metasploit Framework
- Hydra
- Netcat
- KeePassXC
- OpenVAS (Greenbone)
- VirusTotal
- Hybrid Analysis

---

## Target
- **Machine:** Metasploitable2  
- **IP Address:** 192.168.1.6  
- **Environment:** Kali Linux VM  

---

## Workflow Summary
The Red Teaming process followed a structured approach:

1. Reconnaissance  
2. Scanning (Nmap)  
3. Enumeration  
4. Vulnerability Identification  
5. Exploitation (Metasploit)  
6. Privilege Escalation  
7. Password Attacks (Hydra)  
8. Persistence (Cron Jobs)  
9. Reverse Shell (Netcat)  
10. Malware Analysis (EICAR Test File)  
11. Credential Dumping (Mimikatz - Simulated)  
12. MITRE ATT&CK Mapping  
13. Vulnerability Assessment (OpenVAS)  

---

## Repository Structure
Week 1/
│
├── Report/ → Final Security Assessment Report (PDF)
├── Screenshots/ → Evidence of all tasks performed
├── Notes/ → Commands used during testing
└── Workflow/ → Step-by-step Red Teaming methodology


---

## Key Findings
- Multiple critical vulnerabilities were identified
- Weak authentication mechanisms were exploited
- Remote shell access was successfully achieved
- Privilege escalation allowed full system compromise
- Persistence mechanisms ensured continued access

---

## Security Recommendations
- Apply regular patch updates
- Disable unnecessary services
- Enforce strong password policies
- Implement network segmentation
- Enable monitoring and logging

---

## Conclusion
This lab demonstrated how attackers can exploit misconfigured systems using widely available tools. It highlights the importance of proactive security measures and continuous vulnerability assessment.
