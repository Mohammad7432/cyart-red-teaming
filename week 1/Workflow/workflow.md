# Red Teaming Workflow (Week 1)

## 1. Reconnaissance
Initial reconnaissance was performed using Nmap to discover open ports and services on the target system.

## 2. Scanning
Both SYN scan (-sS) and aggressive scan (-A) were used to gather detailed information about services and versions.

## 3. Enumeration
Services such as FTP, SSH, HTTP, SMB, and IRC were identified for further analysis.

## 4. Vulnerability Identification
Known vulnerabilities were mapped to identified services, including UnrealIRCd backdoor and vsftpd vulnerability.

## 5. Exploitation
Metasploit was used to exploit UnrealIRCd and gain remote shell access.

## 6. Privilege Escalation
SUID binaries were identified and exploited to escalate privileges to root.

## 7. Password Attack
Hydra was used to perform brute-force attacks and identify weak credentials.

## 8. Persistence
A cron job was created to maintain access to the system.

## 9. Reverse Shell
Netcat was used to establish a reverse shell connection.

## 10. Malware Analysis
EICAR test file was analyzed using VirusTotal and Hybrid Analysis.

## 11. Credential Dumping
Mimikatz technique was studied conceptually for credential extraction.

## 12. MITRE ATT&CK Mapping
All activities were mapped to MITRE ATT&CK techniques.

## 13. Vulnerability Assessment (OpenVAS)
OpenVAS was used to perform automated vulnerability scanning and validate findings.
