
### Section: Risk Rating Framework & Strategic Remediation Strategy
**Author:** Irfan Musthafa  
**Project Track:** Project 1 (Red Team Assessment & Penetration Test)  
**Deliverables Scope:** CVSS v3.1 Risk Scoring Matrix & 0-180 Day Phased Remediation Roadmap  

---

## 1. Vulnerability Prioritization & CVSS v3.1 Scoring Matrix

The following matrix documents the critical vulnerabilities identified within the Smart Campus infrastructure. Risks have been calculated using the Common Vulnerability Scoring System (CVSS v3.1) framework and prioritized based on operational and business impact severity.

### [V-005] SQL Injection (SQLi) on Student Portal Login
*   **Target Asset:** `Web01` Portal (`203.0.113.10`)[span_0](start_span)[span_0](end_span)[span_1](start_span)[span_1](end_span)
*   **CVSS v3.1 Base Score:** **9.8 (Critical)**[span_2](start_span)[span_2](end_span)[span_3](start_span)[span_3](end_span)
*   **Vector String:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`[span_4](start_span)[span_4](end_span)[span_5](start_span)[span_5](end_span)
*   **Technical Finding:** Analysis of the `portal_access.log` confirms successful authentication bypass via malicious input parameter injection utilizing the `user=admin' OR '1'='1` payload string[span_6](start_span)[span_6](end_span)[span_7](start_span)[span_7](end_span).
*   **Business & Operational Impact:** Grants immediate, unauthenticated administrative access to the underlying relational database management system (RDBMS)[span_8](start_span)[span_8](end_span)[span_9](start_span)[span_9](end_span). This enables full unauthorized extraction, tampering, or deletion of 12,000 student records containing sensitive Personally Identifiable Information (PII) such as Aadhaar numbers, academic grades, and phone data, creating severe legal, financial, and regulatory liabilities[span_10](start_span)[span_10](end_span)[span_11](start_span)[span_11](end_span).

### [V-003] Broken API Authentication (Mass Student Data Dump)
*   **Target Asset:** `API-GW` (`api.hits` / `203.0.113.15`)[span_12](start_span)[span_12](end_span)[span_13](start_span)[span_13](end_span)
*   **CVSS v3.1 Base Score:** **9.1 (Critical)**[span_14](start_span)[span_14](end_span)[span_15](start_span)[span_15](end_span)
*   **Vector String:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N`[span_16](start_span)[span_16](end_span)[span_17](start_span)[span_17](end_span)
*   **Technical Finding:** The API Gateway log (`api_gw.log`) reveals an unauthenticated data exfiltration path where a simple `GET /v1/students` request successfully returns a 2.1MB unauthorized student record dump[span_18](start_span)[span_18](end_span)[span_19](start_span)[span_19](end_span).
*   **Business & Operational Impact:** Exposes the core data layer to automated, scriptable data harvesting without requiring advanced technical exploit mechanics or prior network level permissions[span_20](start_span)[span_20](end_span)[span_21](start_span)[span_21](end_span).

### [V-007] Default Administrative Credentials on Core ERP Enterprise Infrastructure
*   **Target Asset:** `ERP-APP` (`10.20.0.20`) & `ERP-DB` (`10.20.0.21`)[span_22](start_span)[span_22](end_span)[span_23](start_span)[span_23](end_span)
*   **CVSS v3.1 Base Score:** **8.8 (High)**[span_24](start_span)[span_24](end_span)[span_25](start_span)[span_25](end_span)
*   **Vector String:** `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H`[span_26](start_span)[span_26](end_span)[span_27](start_span)[span_27](end_span)
*   **Technical Finding:** Infrastructure scanning identified internal application platforms running factory-default administrative credentials across active deployments[span_28](start_span)[span_28](end_span)[span_29](start_span)[span_29](end_span).
*   **Business & Operational Impact:** Poses an existential threat to operational business continuity[span_30](start_span)[span_30](end_span)[span_31](start_span)[span_31](end_span). Threat actors leveraging these entries gain full administrative leverage over financial registries, procurement workflows, internal human resources documentation, and payroll systems, risking widespread financial fraud[span_32](start_span)[span_32](end_span)[span_33](start_span)[span_33](end_span).

### [V-008] Legacy SMBv1 Active Enlistment & Lateral NTLM Relay Channels
*   **Target Asset:** `AD/LDAP` Domain Controller (`10.20.0.5`) & `Attendance` Subnet (`10.20.0.30`)[span_34](start_span)[span_34](end_span)[span_35](start_span)[span_35](end_span)
*   **CVSS v3.1 Base Score:** **8.1 (High)**[span_36](start_span)[span_36](end_span)[span_37](start_span)[span_37](end_span)
*   **Vector String:** `CVSS:3.1/AV:A/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:N`[span_38](start_span)[span_38](end_span)[span_39](start_span)[span_39](end_span)
*   **Technical Finding:** Identity and access logs (`ad_security.log`) caught active NTLM relay operations originating from the attendance hardware endpoint (`10.20.0.30`) and shifting directly into the core AD infrastructure (`10.20.0.5`)[span_40](start_span)[span_40](end_span)[span_41](start_span)[span_41](end_span).
*   **Business & Operational Impact:** Bypasses internal logical segment boundaries[span_42](start_span)[span_42](end_span)[span_43](start_span)[span_43](end_span). A malicious actor located within the local environment can pivot laterally from insecure edge networks up to active Domain Admin privileges, achieving total control over institutional authentication domains[span_44](start_span)[span_44](end_span)[span_45](start_span)[span_45](end_span).

### [V-004] Insecure Direct Object Reference (IDOR) via Sequential Enumeration
*   **Target Asset:** `HITS Connect` Mobile Application API Gateway[span_46](start_span)[span_46](end_span)[span_47](start_span)[span_47](end_span)
*   **CVSS v3.1 Base Score:** **7.5 (High)**[span_48](start_span)[span_48](end_span)[span_49](start_span)[span_49](end_span)
*   **Vector String:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N`[span_50](start_span)[span_50](end_span)[span_51](start_span)[span_51](end_span)
*   **Technical Finding:** API request logging sequences demonstrate successful parameters enumeration attacks (e.g., sequentially processing `GET /v1/students/10455` through `10457` leveraging standard low-level tokens)[span_52](start_span)[span_52](end_span)[span_53](start_span)[span_53](end_span).
*   **Business & Operational Impact:** Allows authenticated users to sequentially crawl and extract personal profile records of other students, bypassing primary horizontal authorization checking logic[span_54](start_span)[span_54](end_span)[span_55](start_span)[span_55](end_span).

---

## 2. Prioritized Strategic Remediation Roadmap (0–180 Days)

This structured remediation plan establishes phased defensive controls designed to systematically minimize the institution's risk surface before upcoming board audits[span_56](start_span)[span_56](end_span)[span_57](start_span)[span_57](end_span).


### Phase 1: Immediate Emergency Lockdown (0–30 Days)
*   **Active WAF Shielding:** Transition the network's Edge Web Application Firewall (WAF) deployment configurations out of "Monitor Only" mode directly into automated active drop/blocking mode to intercept live web exploits[span_66](start_span)[span_66](end_span)[span_67](start_span)[span_67](end_span).
*   **Critical Input & Endpoint Patching:** Implement immediate source code parameterized queries to remediate the portal login vulnerability (`V-005`) and enforce strict token validation logic on the exposed `/v1/students` endpoint (`V-003`)[span_68](start_span)[span_68](end_span)[span_69](start_span)[span_69](end_span).
*   **Credential Lifecycle Revocation:** Execute a mandatory, institutional password rotation lifecycle targeting default configurations across the internal ERP ecosystem (`V-007`), administrative consoles, and immediately invalidate all API keys exposed via open public `.env` repository leakages[span_70](start_span)[span_70](end_span)[span_71](start_span)[span_71](end_span).
*   **Ingress Network Hardening:** Restrict public internet ingress traffic over Port 22 (SSH) on the Azure Dev/Test sandbox environment and secure open SMB configuration protocols on the internal backup NAS arrays (`10.20.0.99`)[span_72](start_span)[span_72](end_span)[span_73](start_span)[span_73](end_span).

### Phase 2: Foundational Zero-Trust Engineering (30–90 Days)
*   **Network Micro-Segmentation:** Deprecate the existing flat internal network architecture[span_74](start_span)[span_74](end_span)[span_75](start_span)[span_75](end_span). Enforce strict VLAN micro-segmentation, decoupling public-facing DMZ subnets (`VLAN 10`) from sensitive core database infrastructure and corporate identity management directories (`VLAN 20`)[span_76](start_span)[span_76](end_span)[span_77](start_span)[span_77](end_span).
*   **API Security Enforcement:** Deploy secure rate-limiting properties, continuous session token verification routines, and rigorous horizontal Role-Based Access Control (RBAC) layers on all backend infrastructure connections talking to mobile gateway interfaces[span_78](start_span)[span_78](end_span)[span_79](start_span)[span_79](end_span).
*   **Legacy Protocol Depreciation:** Formally disable legacy SMBv1 communication protocols across all internal endpoints and servers to permanently mitigate NTLM relay lateral movements[span_80](start_span)[span_80](end_span)[span_81](start_span)[span_81](end_span).
*   **Identity & Access Management (IAM) Hardening:** Mandate Multi-Factor Authentication (MFA) across all administrative management profiles, server environments, and internal personnel systems[span_82](start_span)[span_82](end_span)[span_83](start_span)[span_83](end_span).

### Phase 3: Strategic Governance & Continuous Monitoring (90–180 Days)
*   **Continuous Vulnerability Operations:** Establish a formal VAPT program cadence, combining routine automated internal credentialed scanner assessments with annual external white-box penetration testing schedules[span_84](start_span)[span_84](end_span)[span_85](start_span)[span_85](end_span).
*   **Centralized Secrets Management:** Migrate clear-text system variables, cryptographic keys, and database connection strings entirely out of static configuration scripts into a dedicated, enterprise-grade secrets vault infrastructure[span_86](start_span)[span_86](end_span)[span_87](start_span)[span_87](end_span).
*   **Supply Chain & Third-Party Auditing:** Institute legally binding secure-coding compliance criteria and technical code audit metrics for all external modules deployed by third-party application vendor teams[span_88](start_span)[span_88](end_span)[span_89](start_span)[span_89](end_span).
*   **Security Information & Event Management (SIEM):** Aggregate firewall logs, authentication records, database access registers, and endpoint telemetry into a centralized logging solution to maintain real-time situational tracking capabilities[span_90](start_span)[span_90](end_span)[span_91](start_span)[span_91](end_span).

