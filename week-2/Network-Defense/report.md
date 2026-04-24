# Network Defense Report – Suricata IDS

## Executive Summary
This task involved creating Suricata IDS rules to detect suspicious network activity. The rules were designed to identify common attacker techniques such as PowerShell usage, command injection, and encoded payload execution.

## Objective
To simulate network-based threat detection using Suricata rules.

## Rules Implemented
- PowerShell detection
- Command execution detection
- Encoded payload detection

## Detection Logic
The rules analyze network traffic for specific patterns such as:
- "powershell"
- "cmd.exe"
- "-enc"

## Analysis
These patterns are commonly associated with malicious activity, including script execution and obfuscation techniques used by attackers.

## Findings
The simulated traffic triggered alerts based on the defined rules, demonstrating how IDS systems can detect suspicious behavior.

## Practical Implementation

Suricata IDS was configured and deployed on Kali Linux. Custom detection rules were written and applied.

Test traffic was generated using curl commands to simulate attacker behavior. The IDS successfully detected the traffic and generated alerts, which were logged in the fast.log file.

Multiple alerts were triggered, confirming that the rules were effective in identifying suspicious network activity.

## Conclusion
Suricata rules provide an effective method for detecting network-based threats. Proper rule design helps identify attacker techniques and enhances network security.
