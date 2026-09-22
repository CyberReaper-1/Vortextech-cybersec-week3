# 🔐 OWASP Juice Shop Security Audit

### VortexTech Cybersecurity Internship — Week 3

> A structured security audit of the intentionally vulnerable OWASP Juice Shop
> application using Docker and OWASP ZAP.

────────────────────────────────────────────

## 📌 Project Overview

- What the project is
- Why OWASP Juice Shop was selected
- Purpose of the security audit
- Assessment scope
- Local/authorized testing statement

## 🎯 Assessment Objectives

- Perform a structured security audit
- Identify common web application vulnerabilities
- Test at least 3 vulnerability categories
- Document findings and evidence
- Analyze potential security impact
- Provide remediation recommendations

## 🧪 Target Environment

| Component | Details |
|-----------|---------|
| Target Application | OWASP Juice Shop |
| Target URL | http://localhost:3000 |
| Deployment | Docker |
| Security Scanner | OWASP ZAP |
| Scan Type | Automated + Manual |
| Environment | Local Practice Lab |
| Assessment | VortexTech Week 3 |

## 🛠️ Tools & Technologies

### Security Testing
- Docker
- OWASP Juice Shop
- OWASP ZAP
- Browser Developer Tools

### Security References
- OWASP Top 10
- CWE
- OWASP Security Guidance

## 🔍 Assessment Methodology

Explain the complete workflow:

1. Deploy Juice Shop
2. Verify application availability
3. Explore application manually
4. Test user inputs
5. Review authentication behaviour
6. Inspect HTTP requests/responses
7. Run OWASP ZAP automated scan
8. Analyze alerts
9. Classify vulnerabilities
10. Document evidence
11. Provide remediation

## 🚀 Lab Setup

### Prerequisites

- Docker Desktop
- OWASP ZAP
- Web Browser

### Deploy OWASP Juice Shop

```bash
docker pull bkimminich/juice-shop
docker run -d -p 3000:3000 bkimminich/juice-shop

Verify:

docker ps

Open:

http://localhost:3000
🕷️ OWASP ZAP Scanning
ZAP configuration
Target configuration
Automated scan process
Passive/active scanning
Report generation
📊 Assessment Results

Summary of all discovered findings.

#	Finding	Severity	Category	CWE
1	Potential SQL Injection	High	Injection	CWE-89
2	Content Security Policy Header Not Set	Medium	Security Misconfiguration	CWE-693
3	CORS Misconfiguration	Medium	Access Control	CWE-264
4	Missing Anti-clickjacking Header	Medium	Security Misconfiguration	CWE-1021
5	Session ID in URL Rewrite	Medium	Session Management	CWE-598
6	Private IP Disclosure	Low	Information Disclosure	CWE-497
7	Timestamp Disclosure – Unix	Low	Information Disclosure	CWE-497
8	X-Content-Type-Options Header Missing	Low	Security Misconfiguration	CWE-693
9	Modern Web Application	Informational	Informational	—
🔎 Detailed Security Findings
1. Potential SQL Injection
Severity
OWASP category
CWE
Affected endpoint
Description
Detection methodology
Payload/request evidence
Observed response
Potential impact
Remediation
Secure implementation example
2. Content Security Policy Header Not Set
Severity
OWASP category
CWE
Affected endpoint
Description
Detection methodology
Evidence
Potential impact
Remediation
Secure configuration example
3. Private IP Disclosure
Severity
OWASP category
CWE
Affected endpoint
Description
Detection methodology
Evidence
Potential impact
Remediation
Secure implementation example
Additional Findings

Brief documentation/reference for the remaining ZAP findings.

🖼️ Evidence
Evidence 1 — Docker Environment

Evidence/...

Evidence 2 — OWASP ZAP Automated Scan

Evidence/...

Evidence 3 — Scan Results

Evidence/...

📑 Reports
Detailed Audit Report

Audit Report/VortexTec-Week3-Audit Report.pdf

OWASP ZAP Scan Report

zap-reports/ZAP-Scan-Report.pdf

Findings Documentation

FINDINGS.md

📁 Repository Structure
Vortextech-cybersec-week3/
│
├── README.md
├── FINDINGS.md
│
├── Audit Report/
│   ├── README.md
│   └── VortexTec-Week3-Audit Report.pdf
│
├── Evidence/
│   ├── 1) Setting up and Running Docker .png
│   ├── 2) Runnng Zap Automated Scan .png
│   └── 3) Automated Scan Results.png
│
└── zap-reports/
    └── ZAP-Scan-Report.pdf
🛡️ Remediation Summary

Consolidated recommendations:

Use parameterized queries
Implement appropriate CSP
Review CORS policies
Add anti-clickjacking protections
Avoid session identifiers in URLs
Remove unnecessary internal information
Implement security headers
Follow least-privilege principles
Perform regular security testing
📚 Security References
OWASP Top 10
OWASP Juice Shop
OWASP SQL Injection Prevention
OWASP CSP Guidance
Relevant CWE entries
RFC 1918
📈 Skills Demonstrated
Web Application Security
Vulnerability Assessment
DAST
OWASP ZAP
OWASP Top 10
CWE Mapping
HTTP Security Analysis
Security Misconfiguration Analysis
Information Disclosure Analysis
Docker-based Security Testing
Vulnerability Documentation
Security Report Writing
📝 Conclusion

Short professional summary covering:

What was assessed
What was discovered
How the findings were analyzed
Why remediation is important
Confirmation that testing was performed in an authorized practice environment
⚠️ Disclaimer

This assessment was performed exclusively against the
OWASP Juice Shop intentionally vulnerable practice application
in a local/authorized laboratory environment.