# Incident Response Report – Suspicious PowerShell Activity

## 1. Executive Summary
A suspicious PowerShell execution was detected through Windows Event Logs. The activity involved the use of ExecutionPolicy Bypass, which is commonly associated with malicious script execution.

## 2. Incident Description
The system generated an Event ID 4688 log indicating a new process creation. The process involved PowerShell executing a script with bypassed execution policy.

## 3. Detection
- Source: Windows Event Viewer
- Event ID: 4688
- Process: powershell.exe
- Command:

powershell -ExecutionPolicy Bypass -File simulate-powershell-activity.ps1

## 4. Analysis
The use of ExecutionPolicy Bypass allows scripts to execute without restriction. This technique is often used by attackers to bypass security controls and execute malicious payloads.

## 5. Indicators of Compromise
- PowerShell execution
- ExecutionPolicy Bypass flag
- Script execution

## 6. MITRE ATT&CK Mapping
- Tactic: Execution
- Technique: T1059.001 (PowerShell)

## 7. Containment Actions
- Monitor and terminate suspicious processes
- Isolate affected system if necessary
- Review logs for additional suspicious activity

## 8. Conclusion
The detected activity is suspicious and demonstrates behavior commonly associated with attacker techniques. Although the activity was part of a controlled simulation, similar patterns in real environments should be treated as potential threats.