# Task 1: OSINT and Reconnaissance

## 1. Subdomain Enumeration
**Tool Used:** Recon-ng (Module: `recon/domains-hosts/hackertarget`)  
**Target:** `tesla.com`


| Subdomain | IP Address | Notes |
| :--- | :--- | :--- |
| ://tesla.com | 23.62.104.69 | Found via HackerTarget API |
| ://tesla.com | 199.232.38.92 | Identified as an Amazon infrastructure asset |
| ://tesla.com | 23.219.36.107 | Identified as a marketing/email tracking server |

## 2. Exposed Services Search
**Tool Used:** Shodan.io  
**Query:** `apache country:US`

**Summary of Exposed Hosts:**
The Shodan search identified over 4.1 million Apache hosts. Specifically, host **54.197.224.100** runs Apache 2.4.62 (Amazon Linux), while **44.246.84.162** handles HTTP 302 redirects. A third host, **104.207.249.79**, exposes a default InterWorx-CP test page. These exposures reveal server versions and internal configurations, aiding attackers in version-specific exploit targeting.
