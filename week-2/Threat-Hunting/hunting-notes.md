# Threat Hunting – Suspicious PowerShell Activity

## Objective
To detect suspicious PowerShell execution activity using Windows process creation logs.

## Data Source
- Windows Security Logs
- Event ID: 4688 (Process Creation)

## Detection Logic
The detection focuses on identifying:
- Execution of powershell.exe
- Suspicious command-line arguments such as:
  - -Command
  - -enc (encoded commands)
  - Invoke-Expression (IEX)
  - DownloadString
  - ExecutionPolicy Bypass

## Hypothesis
Attackers commonly use PowerShell for:
- Executing malicious scripts
- Downloading payloads
- Bypassing security controls

## MITRE ATT&CK Mapping
- Tactic: Execution
- Technique: T1059.001 (PowerShell)
