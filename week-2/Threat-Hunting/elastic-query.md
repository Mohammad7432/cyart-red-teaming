# Detection Query – PowerShell Activity

## Objective
Apply Sigma rule logic to detect PowerShell execution in logs.

## Query Logic
Process Name = powershell.exe  
AND  
Command Line contains:
- -Command
- -ExecutionPolicy
- -enc

## Example Query (Elastic Style)
event.code: 4688 AND process.name: "powershell.exe"

## Suspicious Query
event.code: 4688 AND process.command_line: (*-Command* OR *-ExecutionPolicy* OR *-enc*)

## Alternative Query (Windows Logs)
winlog.event_id: 4688 AND winlog.event_data.NewProcessName: *powershell.exe

## Explanation
These queries replicate the Sigma rule logic by identifying PowerShell execution with suspicious command-line arguments. This helps detect potential attacker activity using PowerShell.
