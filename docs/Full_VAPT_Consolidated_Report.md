# HITS Consolidated VAPT Report (API + Infrastructure + Cloud + IAM)

## 1. Purpose
This report expands beyond API-only issues and includes other critical findings from the dataset in separate sections for clear remediation ownership.

## 2. Scope Sources
1. Case study narrative
2. Scanner-style vulnerability dataset
3. API security documentation and sample requests

## 3. Executive Risk Summary
The environment shows multi-layer risk with several independent paths to data compromise and operational disruption:
1. API-layer access control failures (direct student data exposure)
2. Patch and EOL weaknesses enabling known exploit chains
3. Identity and AD weaknesses enabling privilege escalation
4. Exposed databases and cloud misconfigurations enabling large-scale data leakage
5. Network and remote-access misconfigurations increasing lateral movement risk

## 4. Findings by Section

## Section A: API and Application Security Findings
### Included Findings
1. V-003: API missing auth on `/v1/students` (Critical)
2. V-004: IDOR on `/v1/students/{id}` (High)
3. V-016: No rate limiting on login (High)
4. V-027: JWT `none` algorithm accepted (High)
5. V-050, V-100, V-053: Additional IDOR/BOLA patterns (High)
6. V-005, V-020, V-052, V-065, V-101, V-104: SQL injection across portals/services (Critical/High)
7. V-006, V-055, V-063, V-096, V-105: XSS findings (Medium)
8. V-013, V-089, V-090: Information leakage via verbose/debug behavior (Medium/High)
9. V-103: CSRF in grants application (Medium)
10. V-056: Wallet logic flaw (High)

### Why this section matters
These issues enable direct compromise of student records and transaction integrity through internet-facing application paths.

### Primary owners
1. Application Engineering
2. API Platform Team
3. DevSecOps

---

## Section B: Server Patching, EOL, and Known-CVE Exposure
### Included Findings
1. V-001, V-019: Apache path traversal/RCE class exposures (Critical)
2. V-002, V-062: Outdated/EOL web servers (High)
3. V-021: GitLab CE RCE outdated (Critical)
4. V-024: vCenter unauth RCE (Critical)
5. V-035, V-036: Exchange ProxyLogon/ProxyShell (Critical)
6. V-031, V-032: ESXi RCE exposure (High)
7. V-073, V-075: BlueKeep/PrintNightmare risk (Critical/High)
8. V-092: Veeam deserialization RCE (High)
9. V-046, V-106, V-108: Unsupported EOL operating systems (Critical/High)

### Why this section matters
Publicly known CVEs with available exploit tooling significantly reduce attacker effort and increase ransomware risk.

### Primary owners
1. Infrastructure Operations
2. Server Platform Team
3. Vulnerability Management Team

---

## Section C: Identity, Access, and Active Directory Weaknesses
### Included Findings
1. V-007, V-012, V-025, V-054, V-064, V-069, V-091: Default/weak credentials (High/Critical)
2. V-028: Kerberoastable service accounts (High)
3. V-029: GPP `cpassword` in SYSVOL (High)
4. V-094, V-095: Zerologon exposure (Critical)
5. V-034: AD CS ESC1 misconfiguration (High)

### Why this section matters
Identity weaknesses can turn one low-level foothold into domain-wide compromise.

### Primary owners
1. IAM Team
2. AD/Windows Engineering
3. Security Operations

---

## Section D: Network and Perimeter Security Findings
### Included Findings
1. V-008, V-047: SMBv1 / EternalBlue class exposure (Critical)
2. V-009: Open anonymous SMB share (High)
3. V-010: SSH password auth publicly exposed (High)
4. V-072, V-074: RDP hardening gaps and SMB signing disabled (High)
5. V-076: DNS zone transfer allowed (Medium)
6. V-081: Internal firewall permissive any-any rule (High)
7. V-080: Edge router weak SSH ciphers (Medium)
8. V-011, V-033: VPN/Firewall critical CVE exposure (Critical)

### Why this section matters
Weak segmentation and legacy protocols increase lateral movement and blast radius after initial compromise.

### Primary owners
1. Network Engineering
2. Firewall/VPN Operations
3. SOC and Detection Engineering

---

## Section E: Database, Secrets, and Data Exposure
### Included Findings
1. V-018: `xp_cmdshell` enabled on MSSQL (High)
2. V-026, V-038, V-039, V-048, V-083: Legacy or unauthenticated DB services (Critical/High)
3. V-015, V-051, V-057, V-058: Cleartext/weak cryptography for sensitive data (High)
4. V-017, V-093, V-110: Hardcoded keys/cleartext credentials/secrets (High)
5. V-088: Dev database with production data exposed (High)

### Why this section matters
These findings directly increase risk of mass data theft and regulated-data non-compliance.

### Primary owners
1. DBA Team
2. Data Platform Engineering
3. Application Security

---

## Section F: Cloud and DevOps Misconfiguration Findings
### Included Findings
1. V-037: S3 bucket public write (Critical)
2. V-082: Publicly accessible RDS (High)
3. V-085: Over-privileged Lambda IAM role (High)
4. V-040: Docker API exposed (Critical)
5. V-041: Kubernetes dashboard no auth (Critical)
6. V-084: Hadoop UI exposed (High)
7. V-086, V-087: Repository/artifact anonymous access (High)

### Why this section matters
Cloud misconfiguration and CI/CD exposure can enable rapid data tampering, backdoor placement, and supply-chain abuse.

### Primary owners
1. Cloud Platform Team
2. DevOps/SRE
3. CI/CD Governance Team

---

## Section G: IoT/OT and Physical-Security Adjacent Systems
### Included Findings
1. V-023: CCTV/NVR auth bypass class issue (Critical)
2. V-059: Door controller default creds (High)
3. V-060: MQTT broker no authentication (High)
4. V-061: BACnet exposure (Medium)
5. V-111 to V-114 class from dataset context (biometric/edge control risks reflected by V-058, V-059)

### Why this section matters
Compromise of smart-campus OT/IoT can impact safety, physical access, and trust in attendance systems.

### Primary owners
1. OT/Facilities IT
2. Network Segmentation Team
3. Security Architecture

## 5. Unified Prioritization (Cross-Section)

### P1 Immediate (0-14 days)
1. Close unauthenticated API data access (V-003)
2. Fix IDOR/BOLA in student and related APIs (V-004, V-050, V-053, V-100)
3. Patch internet-exposed critical CVEs (V-033, V-035, V-036, V-024, V-021, V-001/V-019)
4. Remove public write/read on cloud storage and admin surfaces (V-037, V-041, V-040)
5. Disable legacy critical protocols/services (SMBv1, vulnerable RDP paths) (V-008, V-047, V-073)
6. Remediate Zerologon and AD privilege escalation paths (V-094, V-095, V-034)

### P1 High (15-45 days)
1. Enforce centralized authz, token hardening, and login anti-automation (V-016, V-027, V-098)
2. Eliminate default/weak credentials across ERP/DB/admin systems (V-007, V-012, V-025, V-054, V-064, V-069, V-091)
3. Restrict DB/network exposure and remove anonymous shares (V-009, V-068, V-082, V-083)
4. Remove cleartext secrets/credentials and enforce KMS/Vault controls (V-017, V-093, V-110)

### P2 Medium (46-90 days)
1. Reduce info leakage/debug exposure and tighten secure headers/error handling (V-013, V-089, V-090)
2. Resolve medium-severity web flaws (XSS, CSRF, open redirect) with SDLC controls (V-055, V-063, V-096, V-102, V-103)
3. Complete EOL decommission and lifecycle governance (V-046, V-106, V-108)

## 6. Owner-Based Action Matrix
| Workstream | Example Findings | Owner | Target Window |
|---|---|---|---|
| API/Auth hardening | V-003, V-004, V-016, V-027 | App Engineering + AppSec | 0-30 days |
| Critical patching | V-033, V-035, V-036, V-024, V-021 | Infra Ops + Vuln Mgmt | 0-21 days |
| AD/IAM remediation | V-028, V-029, V-094, V-095, V-034 | IAM + AD Team | 0-30 days |
| Network segmentation | V-008, V-009, V-081, V-074 | Network Team | 0-45 days |
| Cloud misconfig fixes | V-037, V-082, V-085, V-041 | Cloud Platform + SRE | 0-30 days |
| Data/security controls | V-015, V-017, V-057, V-093 | DBA + Security Engineering | 15-60 days |

## 7. Governance and Verification Plan
1. Run weekly remediation tracking with evidence-based closure.
2. Require retest for every P1 closure before sign-off.
3. Add CI/CD security gates for API authz tests and secret scanning.
4. Implement patch SLAs by severity (Critical <= 14 days, High <= 30 days).
5. Present monthly board dashboard with risk-burndown and reopened findings.

## 8. Final Conclusion
The dataset indicates that risk is not limited to API logic flaws. The most realistic attack scenarios chain API weaknesses with patching gaps, IAM misconfigurations, and exposed infrastructure. The organization should execute a coordinated cross-team remediation program using the sectioned ownership model in this report.
