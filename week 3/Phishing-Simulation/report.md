# Task 2: Phishing Simulation

## 1. Project Overview
**Objective:** Simulate a credential harvesting attack to demonstrate how initial access can be gained through social engineering.
**Tools Used:** Social Engineering Toolkit (SET), Kali Linux.

## 2. Methodology
1. **Site Cloning:** Used SET's `Site Cloner` module to create a local replica of a common login portal.
2. **Interception:** Configured a Credential Harvester to listen on the attacker's local IP (127.0.0.1).
3. **Execution:** Intercepted a POST request containing the victim's login identifiers and password data.

## 3. Credential Harvest Log


| Timestamp           | IP Address   | Username           | Password (Status)  | Risk |
|---------------------|--------------|--------------------|--------------------|------|
| 2025-08-29 12:00:00 | 127.0.0.1    | victim@test.com    | [Encrypted/Captured] | High |

**Technical Note:** The password was captured in an obfuscated/encrypted format typical of modern web frameworks. In a real engagement, this would be used for offline cracking or "Pass-the-Hash" attacks.
