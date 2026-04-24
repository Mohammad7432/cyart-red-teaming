# Incident Response Report – Phishing Attack

## 1. Executive Summary
A phishing attack was identified where an employee received a malicious email containing an attachment. Upon opening the attachment, suspicious system activity was observed, indicating potential compromise.

## 2. Incident Description
An employee reported receiving an email that appeared legitimate but contained a malicious attachment. After opening the attachment, the system began showing unusual behavior.

## 3. Detection
- Source: User report
- Indicators:
  - Suspicious email attachment
  - Unknown processes running
  - Unusual system behavior

## 4. Analysis
The attachment likely contained malicious code that executed upon opening. This may have resulted in unauthorized system access and potential data exfiltration.

## 5. Impact
- Potential compromise of user system
- Risk of data leakage
- Possible lateral movement within the network

## 6. MITRE ATT&CK Mapping
- Initial Access: Phishing (T1566)
- Execution: User Execution (T1204)

## 7. Containment Actions
- Isolated affected system from network
- Disabled compromised user account
- Blocked malicious email source

## 8. Eradication
- Removed malicious files
- Scanned system for malware
- Applied security patches

## 9. Recovery
- Restored system functionality
- Verified system integrity
- Re-enabled user access

## 10. Lessons Learned
- Improve email filtering
- Conduct user awareness training
- Enhance monitoring and detection

## 11. Conclusion
The phishing attack demonstrates the importance of user awareness and proactive monitoring. Proper incident response helped contain and mitigate the threat effectively.