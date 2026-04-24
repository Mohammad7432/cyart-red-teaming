# Capstone Project Report – PowerShell Attack Simulation

## 1. Executive Summary
A simulated attack involving suspicious PowerShell execution was conducted to demonstrate detection, analysis, and incident response capabilities. The activity was identified as potentially malicious due to the use of ExecutionPolicy Bypass.

## 2. Scenario
A PowerShell command was executed with bypassed execution policy to run a script. This behavior is commonly associated with attacker techniques.

## 3. Detection
The activity was detected using Windows Event Viewer logs (Event ID 4688). The command involved powershell.exe with ExecutionPolicy Bypass.

## 4. Analysis
The command indicates script execution without restrictions. This aligns with MITRE ATT&CK technique T1059.001 (PowerShell).

## 5. Indicators of Compromise
- PowerShell execution
- ExecutionPolicy Bypass
- Script execution

## 6. Incident Response
- System isolation
- Process termination
- Log analysis
- Malware scanning

## 7. Risk Assessment
The risk was assessed as Medium based on likelihood and impact. The estimated annual loss (ALE) was $1500.

## 8. Conclusion
The simulation demonstrates how PowerShell-based attacks can be detected and mitigated. Proper monitoring and response strategies are essential for maintaining system security.