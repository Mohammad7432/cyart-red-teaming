# Red Team Lab Report - Week 4: Cloud & Adversary Emulation

## 1. Executive Summary
This report documents the results of a simulated cloud environment attack (Task 2) performed in a controlled lab setting using Moto and Pacu. The assessment successfully identified critical vulnerabilities in S3 bucket configurations and IAM permission structures.

---

## 2. Task 2: Cloud Attack Lab Documentation

### A. Cloud Reconnaissance (AID001)
**Objective:** Identify publicly accessible storage assets.
**Actions:** Performed enumeration using `awscli` and Pacu's `s3__enum` module against the mock cloud endpoint.


| Asset ID | Service | Misconfiguration     | Notes                                      |
|----------|---------|---------------------|--------------------------------------------|
| AID001   | S3      | Public read access  | Found 'aid001-vulnerable-bucket' via CLI.  |

**Evidence:**
- Bucket identified: `s3://aid001-vulnerable-bucket`
- Permissions confirmed: `AllUsers` - `READ`

### B. Privilege Escalation (AID002)
**Objective:** Identify and exploit overprivileged IAM identities.
**Actions:** Utilized Pacu to analyze the `vulnerable-user`. Discovered an inline "FullAdmin" policy allowing global AWS actions.


| Attack ID | Service | Misconfiguration      | Notes                                  |
|-----------|---------|----------------------|----------------------------------------|
| AID002    | IAM     | Overprivileged role  | User had '*' permissions on all resources.|

**Privilege Escalation Summary (50 Words):**
Using Pacu's `iam__enum_permissions`, I identified a critical IAM misconfiguration where the `vulnerable-user` was granted an inline `FullAdmin` policy. By exploiting these excessive permissions, I successfully performed unauthorized administrative actions across the simulated environment, demonstrating a complete compromise of the cloud identity boundary and security controls.

### C. Exfiltration Log
**Objective:** Confirm the ability to extract sensitive data.
**Actions:** Downloaded mock customer data from the vulnerable S3 bucket without valid credentials.


| Phase       | Tool Used | Action Description | MITRE Technique |
|-------------|-----------|--------------------|-----------------|
| Recon       | Pacu      | S3 bucket enum     | T1580           |
| Exfiltration| AWS CLI   | Data retrieval     | T1020           |

**Confirmation:** Receipt of `data.txt` confirmed in local logs. Mock data: `CREDIT_CARD_DATA_MOCK`.

---

## 3. Remediation Recommendations
1. **S3 Hardening:** Block Public Access at the account level and implement "Least Privilege" bucket policies.
2. **IAM Hygiene:** Conduct monthly audits for "AdministratorAccess" and replace inline policies with managed, restricted policies.
3. **MFA Enforcement:** Require Multi-Factor Authentication for all IAM users, especially those with destructive permissions.
