# Alerts Analysis

## Practical Detection

Suricata IDS was successfully configured and executed on Kali Linux. Custom rules were applied to detect suspicious patterns in network traffic.

## Detected Alerts

### 1. PowerShell Download Detected
- Indicates possible malicious script download activity

### 2. Encoded Command Detected
- Indicates use of obfuscated or encoded payloads
- Common attacker technique

### 3. Command Execution Detected
- Indicates attempt to execute system-level commands

## Log Evidence
The alerts were recorded in Suricata's fast.log file and showed multiple detections triggered by generated traffic.

## Conclusion
The practical implementation successfully demonstrated real-time network threat detection using Suricata IDS. The rules effectively identified suspicious traffic patterns associated with attacker behavior.
