# Threat Hunting Findings

|      Timestamp      |     Source    |    Process     | Command Line | Detection Reason | MITRE Technique | Notes |
|---------------------|---------------|----------------|-----------------------------------------------------------------------------|---------------------------------|-----------------|-------|
| 2026-04-24 10:11:37 | Event ID 4688 | powershell.exe | powershell -ExecutionPolicy Bypass -File .\simulate-powershell-activity.ps1 | ExecutionPolicy Bypass detected | T1059.001 | Suspicious pattern (lab simulation) |
