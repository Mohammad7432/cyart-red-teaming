# Threat Hunting Report – Suspicious PowerShell Activity

## 1. Executive Summary
This task focused on detecting suspicious PowerShell activity using Windows process creation logs, Sigma detection logic, and Elastic-style query logic. A harmless PowerShell simulation was executed in a Windows lab environment to generate process creation events. The activity was successfully detected and mapped to MITRE ATT&CK technique T1059.001.

## 2. Objective
The objective was to identify suspicious PowerShell activity by:
- Creating a Sigma detection rule
- Generating PowerShell execution events
- Analyzing Windows Event ID 4688 logs
- Applying detection logic using queries
- Documenting findings

## 3. Tools Used
- Windows Virtual Machine
- PowerShell
- Event Viewer
- AuditPol
- Sigma Rules
- Elastic Query Logic

## 4. Detection Logic
The Sigma rule detects PowerShell execution based on:
- Process name ending with powershell.exe
- Command-line arguments indicating suspicious activity such as:
  - -Command
  - -enc
  - Invoke-Expression (IEX)
  - DownloadString
  - ExecutionPolicy Bypass

## 5. Simulation Performed
A benign PowerShell script was created and executed to simulate activity. The following command was used:

powershell -ExecutionPolicy Bypass -File .\simulate-powershell-activity.ps1

This generated a process creation event in Windows logs.

## 6. Evidence Collected
The following evidence was collected:
- Screenshot of Sigma rule
- Screenshot of audit policy configuration
- Screenshot of PowerShell script
- Screenshot of PowerShell execution output
- Screenshot of Event Viewer showing Event ID 4688
- Screenshot of query logic

## 7. Log Analysis
The Windows Event Viewer captured a process creation event (Event ID 4688) showing the execution of PowerShell.

The process command line included:
powershell -ExecutionPolicy Bypass -File simulate-powershell-activity.ps1

This is significant because attackers commonly use the ExecutionPolicy Bypass flag to execute scripts without restriction, bypassing security controls.

The observed behavior matches the Sigma rule conditions and demonstrates how suspicious PowerShell activity can be detected through log analysis.

## 8. Detection Logic Validation
The Sigma rule was validated using real system logs. The rule detects:
- powershell.exe execution
- suspicious command-line arguments such as ExecutionPolicy Bypass

The generated log confirms that the detection logic is correct, as the simulated PowerShell activity triggered the expected log pattern.

## 9. MITRE ATT&CK Mapping
- Tactic: Execution
- Technique: T1059.001 – PowerShell

## 10. Conclusion
This task demonstrated how threat hunting can be performed using process creation logs and detection logic. Sigma rules provide a standardized method for defining detections, while query logic helps identify suspicious activity in logs. The simulation confirmed that PowerShell-based attack techniques can be detected effectively using Event ID 4688.
