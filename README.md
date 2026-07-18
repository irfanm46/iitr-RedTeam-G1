________________________________________
RED TEAM ASSESSMENT & PENETRATION TEST REPORT
Smart Campus Security Assessment
Prepared For
Himalaya Institute of Technology & Sciences (HITS)
________________________________________
Client -
Himalaya Institute of Technology & Sciences (HITS)
________________________________________
Assessment Type
Red Team Assessment & Vulnerability Assessment & Penetration Testing (VAPT)
________________________________________
Assessment Standards
•	Penetration Testing Execution Standard (PTES)
•	NIST SP 800-115 Technical Guide to Information Security Testing
•	OWASP Web Security Testing Guide
•	MITRE ATT&CK Framework
•	CVSS v3.1 Risk Rating
________________________________________
Prepared By
 Irfan Musthafa ,Swannd Belsare,Shaikh Ezaz, Mumpi ,Rajeev Gorai ,Siddhi Jain
Masai School × IIT Roorkee Cyber Security Program
________________________________________
________________________________________
Report Date
July 2026
________________________________________
CONFIDENTIAL
This report contains confidential security information intended solely for the management of Himalaya Institute of Technology & Sciences (HITS). The findings and recommendations presented are based on a simulated assessment using the supplied case study, architecture diagrams, and sample logs. This document must not be distributed without authorization.
________________________________________
Letter of Authorization
Purpose
This document summarizes the results of a simulated Red Team Assessment and Vulnerability Assessment & Penetration Testing (VAPT) conducted for the HITS Smart Campus environment.
The engagement was performed exclusively within the scope defined by the academic project. All findings are based on the supplied network architecture, system logs, and supporting documentation. No live systems were scanned, exploited, or modified during this assessment.
The objective was to identify potential attack paths, evaluate security weaknesses, estimate business impact, and recommend effective remediation strategies.
________________________________________
Document Control
Item	Details
Project	Smart Campus Red Team Assessment
Client	Himalaya Institute of Technology & Sciences
Assessment Type	Simulated VAPT
Prepared By	Irfan Musthafa ,Swannd Belsare,Shaikh Ezaz,Mumpi ,Rajeev Gorai, Siddhi Jain 
Classification	Confidential
Status	Final
date	
July 2026

________________________________________
________________________________________
Table of Contents
1.	Executive Summary
2.	Client Background
3.	Project Objectives
4.	Scope of Assessment
5.	Rules of Engagement
6.	Assessment Methodology
7.	Smart Campus Architecture
8.	Asset Inventory
9.	Network Enumeration
10.	Vulnerability Assessment
11.	Web Application Security Assessment
12.	API Security Assessment
13.	Active Directory Security Assessment
14.	Firewall Security Assessment
15.	Attack Chain Analysis
16.	MITRE ATT&CK Mapping
17.	Risk Assessment
18.	Cyber Risk Dashboard
19.	Remediation Roadmap
20.	Conclusion
________________________________________
Executive Summary
Overview
Himalaya Institute of Technology & Sciences (HITS) is a modern university with approximately 12,000 students. Over the last three years, the university invested nearly ₹18 crore to implement a Smart Campus ecosystem that integrates academic, administrative, and operational services into a centralized digital environment.
The Smart Campus infrastructure includes:
•	Student Portal
•	Enterprise Resource Planning (ERP)
•	Hostel Management System
•	RFID & Face Recognition Attendance System
•	Mobile Application (HITS Connect)
•	Active Directory Infrastructure
•	Cloud-hosted Reporting Services
Following an anonymous claim on a hacker forum alleging the sale of 12,000 student records, university management initiated a security assessment to determine whether attackers could realistically compromise sensitive student information or disrupt university operations.
This report presents the findings of a simulated Red Team Assessment based on the provided case study, network diagram, API documentation, and sample security logs.
________________________________________
Assessment Objectives
The primary objectives of this engagement were to:
•	Identify publicly exposed assets.
•	Analyze the network architecture and attack surface.
•	Assess potential vulnerabilities in web applications and APIs.
•	Review authentication and Active Directory security events.
•	Analyze firewall exposure.
•	Simulate a realistic attack chain using the available evidence.
•	Prioritize risks using the CVSS framework.
•	Recommend practical remediation measures.
________________________________________
Assessment Scope
The assessment covered the following systems:
System	Purpose
Student Portal	Academic Services
ERP	Finance, HR, Procurement
Hostel Management	Student Accommodation
Attendance System	RFID & Face Recognition
Mobile API	Mobile Application Backend
Active Directory	Authentication Services
Cloud Reports Server	Reporting Infrastructure
Firewall	Network Security
________________________________________
Assessment Methodology
The assessment followed widely recognized cybersecurity standards:
•	PTES (Penetration Testing Execution Standard)
•	NIST SP 800-115
•	OWASP Web Security Testing Guide
•	MITRE ATT&CK Framework
•	CVSS v3.1
________________________________________
Executive Findings
Analysis of the supplied evidence identified several critical security weaknesses that could significantly affect the confidentiality, integrity, and availability of university systems.
Major Findings
Finding	Severity
SQL Injection	Critical
Broken Access Control	Critical
Insecure Direct Object Reference (IDOR)	Critical
Sensitive Data Exposure	Critical
Cross-Site Scripting (XSS)	High
Brute Force Login Attempts	High
NTLM Relay Attempt	Critical
World-Accessible SSH Service	High
________________________________________
Overall Risk Rating
Overall Security Risk: HIGH
The assessment indicates that an attacker could potentially:
•	Obtain unauthorized administrative access.
•	Access confidential student records.
•	Enumerate sensitive API resources.
•	Exploit weak network segmentation.
•	Move laterally within the internal network.
•	Target privileged Active Directory accounts.
•	Access cloud infrastructure through exposed services.
Although the assessment was conducted using simulated evidence, the identified weaknesses demonstrate realistic attack paths that require prompt remediation.
________________________________________
Business Impact
If these vulnerabilities were exploited in a real-world environment, the potential consequences include:
•	Exposure of student personal information.
•	Loss of academic records and examination data.
•	Financial fraud involving ERP systems.
•	Unauthorized access to hostel and attendance services.
•	Reputational damage to the university.
•	Possible regulatory action and financial penalties under applicable data protection laws.
________________________________________
Recommendations Summary
Immediate priority should be given to:
•	Remediating SQL Injection vulnerabilities.
•	Enforcing authentication and authorization for all API endpoints.
•	Eliminating IDOR vulnerabilities through server-side access validation.
•	Restricting SSH access to trusted administrative networks.
•	Implementing network segmentation between public-facing and internal systems.
•	Strengthening Active Directory security controls.
•	Deploying continuous security monitoring and log analysis.
________________________________________
Report Structure
This report is organized into the following sections:
1.	Executive Summary
2.	Assessment Methodology
3.	Network Enumeration
4.	Vulnerability Assessment
5.	Web Application Security
6.	API Security
7.	Active Directory Assessment
8.	Firewall Assessment
9.	Attack Chain Analysis
10.	Risk Assessment
11.	Remediation Roadmap
12.	Conclusion
________________________________________
Statement of Authenticity
This report has been prepared as part of an academic cybersecurity project. All findings, observations, and recommendations are derived from the provided case study, network diagram, and sample logs. The report does not represent testing of a live production environment and should be interpreted as a simulated professional security assessment.
________________________________________

