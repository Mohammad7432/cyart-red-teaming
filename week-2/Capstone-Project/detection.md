# Detection

## Method
The suspicious activity was detected using Windows Event Viewer logs.

## Log Details
- Event ID: 4688 (Process Creation)
- Process: powershell.exe

## Command Detected
powershell -ExecutionPolicy Bypass -File simulate-powershell-activity.ps1

## Detection Logic
The detection was based on identifying:
- PowerShell execution
- ExecutionPolicy Bypass flag

## Sigma Rule Reference
The activity matches Sigma rule conditions for suspicious PowerShell execution.