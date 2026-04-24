# Week 2 – Cybersecurity Practical Labs

## Overview

This repository contains hands-on cybersecurity tasks completed as part of Week 2 of the CYART Red Teaming program. The focus of this week was to build practical skills across multiple domains including threat hunting, malware analysis, vulnerability management, incident response, risk assessment, and network defense.

---

## Objectives

* Detect suspicious system activity using logs
* Perform static malware analysis
* Identify and remediate vulnerabilities
* Simulate incident response workflows
* Conduct risk assessment
* Implement network-based threat detection
* Build an end-to-end cybersecurity workflow

---

## Tasks Completed

### 1. Threat Hunting

* Used Windows Event Viewer (Event ID 4688)
* Detected suspicious PowerShell activity
* Created Sigma rules for detection
* Identified malicious execution patterns

---

### 2. Malware Analysis

* Analyzed `calc.exe` using static analysis
* Extracted strings from binary
* Identified DLL imports and API calls
* Verified file as benign using Hybrid Analysis

---

### 3. Vulnerability Management

* Performed vulnerability scan using OpenVAS (Greenbone)
* Identified SSL/TLS weak cipher vulnerability
* Assessed risk severity
* Developed remediation plan

---

### 4. Incident Response Simulation

* Simulated PowerShell-based attack
* Identified Indicators of Compromise (IOCs)
* Mapped activity to MITRE ATT&CK (T1059.001)
* Performed containment and response actions

---

### 5. Network Defense (Suricata IDS)

* Configured Suricata on Kali Linux
* Created custom IDS rules
* Generated network traffic using `curl`
* Successfully triggered alerts in `fast.log`
* Demonstrated real-time network threat detection

---

### 6. Risk Assessment

* Performed qualitative and quantitative risk analysis
* Calculated ALE (Annual Loss Expectancy)
* Built risk matrix
* Evaluated impact and likelihood

---

### 7. Incident Report (Phishing Scenario)

* Simulated phishing attack scenario
* Documented detection, analysis, and response
* Identified IOCs and attack impact
* Created structured incident report

---

### 8. Capstone Project

* Simulated full attack lifecycle:

  * Detection → Analysis → Response → Risk Assessment
* Integrated all previous tasks into one workflow
* Demonstrated end-to-end cybersecurity process

---

## Tools Used

* Windows Event Viewer
* PowerShell
* Sigma Rules
* OpenVAS (Greenbone)
* Suricata IDS
* Kali Linux
* Hybrid Analysis

---

## Repository Structure

```bash
week-2/
├── Threat-Hunting/
├── Malware-Analysis/
├── Vulnerability-Management/
├── Incident-Response-Simulation/
├── Risk-Assessment/
├── Incident-Report/
├── Capstone-Project/
├── Network-Defense/
└── README.md
```

---

## Screenshots

Each task folder contains relevant screenshots demonstrating:

* Detection logs
* Tool outputs
* Alerts and findings
* Analysis results

---

## Key Learnings

* Identifying attacker behavior using logs
* Understanding malware through static analysis
* Detecting vulnerabilities and applying fixes
* Responding to security incidents effectively
* Performing structured risk assessments
* Implementing network-based detection systems

---

## Conclusion

This project demonstrates a comprehensive cybersecurity workflow, covering detection, analysis, response, and prevention. It highlights the importance of combining multiple security approaches to effectively defend systems against threats.
