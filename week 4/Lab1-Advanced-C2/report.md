# Lab 1: Advanced C2 Lab – Infrastructure & Management

## 1. Executive Summary
The primary objective of this lab was to establish a Command and Control (C2) infrastructure to manage a remote Windows host. While the initial brief specified Cobalt Strike or PoshC2, environmental dependencies (specifically Python OpenSSL conflicts) and licensing restrictions led to the selection of **Sliver C2** as the primary framework. By leveraging a **Cloudflare Tunnel**, the lab successfully demonstrated "Global" C2 capabilities, bypassing local NAT restrictions to control a target on an external network.

## 2. Technical Infrastructure

| Component | Tool / Value |
| :--- | :--- |
| **C2 Framework** | Sliver C2 (Golang-based) |
| **Tunneling Service** | Cloudflare Tunnel (cloudflared) |
| **Listener Type** | HTTPS (Port 443) |
| **Public Endpoint** | https://trycloudflare.com |
| **Target OS** | Windows 10/11 |

## 3. Workflow & Implementation
1. **Global Bridge Setup:** Initiated a `cloudflared` tunnel to create a public HTTPS endpoint, mapping external traffic to the local Sliver listener.
2. **Framework Transition:** Due to persistent library errors with PoshC2 and the commercial nature of Cobalt Strike, the **Sliver C2** framework was deployed for its robust, stageless implant capabilities.
3. **Payload Customization:** Generated a customized Go-based implant configured to communicate with the Cloudflare URL.
4. **Execution:** The payload (`implant.exe`) was deployed and executed on a remote target.
5. **Session Management:** Successfully interacted with the remote session, performing reconnaissance tasks (`info`, `whoami`) and verifying encrypted communication.

## 4. Session Log

| Session ID | Target IP | Payload Type | Notes |
| :--- | :--- | :--- | :--- |
| SID001 | [Remote_IP] | Sliver EXE | Established via Cloudflare Tunnel |

## 5. Lab Summary (50-Word)
The C2 infrastructure was established using the Sliver framework bridged through a Cloudflare HTTPS tunnel. A customized implant was generated and executed on a remote Windows target. The session was successfully managed via the Sliver handler, confirming robust remote command-and-control capability and the ability to bypass local network restrictions.

## 6. Remediation Recommendations
- **Detection:** Monitor for outbound HTTPS traffic to common tunneling services (e.g., `*.trycloudflare.com`, `*.ngrok.io`).
- **Prevention:** Implement Application Whitelisting (AppLocker) to prevent the execution of unsigned binaries in user-writable directories.
- **Network:** Use SSL/TLS inspection to identify malicious C2 traffic patterns hidden within encrypted channels.
