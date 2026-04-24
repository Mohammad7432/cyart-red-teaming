\# Incident Notes – Suspicious PowerShell Activity



\## Incident Description

A suspicious PowerShell execution was detected on the system using Event Viewer logs.



\## Detection Source

\- Windows Event Logs

\- Event ID: 4688 (Process Creation)



\## Observed Activity

The following command was executed:



powershell -ExecutionPolicy Bypass -File simulate-powershell-activity.ps1



\## Initial Assessment

The use of ExecutionPolicy Bypass is suspicious because it allows scripts to run without restriction. This behavior is commonly associated with malicious activity.



\## Severity

Medium (based on suspicious execution pattern)

