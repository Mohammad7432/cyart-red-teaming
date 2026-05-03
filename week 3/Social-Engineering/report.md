# Task 5: Social Engineering Lab

## 1. Intel Gathering Log
**Objective:** Perform Open Source Intelligence (OSINT) gathering to build a profile for a targeted social engineering attack.
**Tools Used:** PhoneInfoga, Maltego.


| Target ID | Data Source | Information | Notes |
| :--- | :--- | :--- | :--- |
| TID001 | PhoneInfoga | Carrier: Verizon; Type: Mobile | Verified active line via local scanners |
| TID001 | Google Dorks | Social Media Footprints | Linked target to LinkedIn and Facebook |
| TID001 | Maltego | Relationship Mapping | Connected phone to corporate email domain |

## 2. Vishing Simulation Summary
**Scenario Summary:**
"Posing as an IT Support specialist from the target's mobile carrier, I contacted the victim regarding a 'security synchronization' error. Using the carrier data from PhoneInfoga to establish trust, I persuaded the target to visit a cloned login portal to 're-verify' their credentials, successfully capturing their corporate password." (50 words)

## 3. Methodology
1. **Footprinting:** Used PhoneInfoga to identify the carrier and location of the target number (+1-555-0199).
2. **Dorking:** Executed automated Google Dorks to find the target's presence on public directories and social media.
3. **Graphing:** Utilized Maltego to visually map the relationships between the phone number, known aliases, and professional affiliations to craft a high-success pretext.
