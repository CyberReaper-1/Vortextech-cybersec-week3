# 🔐 OWASP Juice Shop Security Audit

### VortexTech Cybersecurity Internship — Week 3

A structured security audit of the intentionally vulnerable **OWASP Juice Shop** web application using **Docker** and **OWASP ZAP**.

---

## 📌 1. Project Overview

### What is the Project?

This project involves performing a security audit of the **OWASP Juice Shop**, an intentionally vulnerable web application designed for cybersecurity training and security testing.

The assessment combines **manual security testing** with **automated vulnerability scanning using OWASP ZAP**.

### Why OWASP Juice Shop?

OWASP Juice Shop was selected because it contains a wide range of realistic web application vulnerabilities mapped to common security standards such as the **OWASP Top 10**.

### Purpose of the Audit

The purpose of this assessment is to:

* Identify common web application security vulnerabilities
* Understand how vulnerabilities can be detected
* Collect and document security evidence
* Analyze the potential impact of identified issues
* Provide appropriate remediation recommendations

### Assessment Scope

The assessment is limited to the locally deployed OWASP Juice Shop application:

**Target:** `http://localhost:3000`

Testing was performed only against the intentionally vulnerable application running in the local laboratory environment.

---

## 🎯 2. Assessment Objectives

The primary objectives of this assessment are to:

1. Perform a structured web application security audit
2. Identify at least **three vulnerability categories**
3. Perform both automated and manual security testing
4. Document discovered vulnerabilities with supporting evidence
5. Analyze the security impact of each finding
6. Map findings to relevant security classifications where applicable
7. Provide practical remediation recommendations

---

## 🧪 3. Target Environment

| Component          | Details                                      |
| ------------------ | -------------------------------------------- |
| Target Application | OWASP Juice Shop                             |
| Target URL         | `http://localhost:3000`                      |
| Deployment Method  | Docker                                       |
| Security Scanner   | OWASP ZAP                                    |
| Scan Type          | Automated + Manual                           |
| Environment        | Local Practice Lab                           |
| Assessment         | VortexTech Cybersecurity Internship — Week 3 |

---

## 🛠️ 4. Tools & Technologies

### Security Testing Tools

* Docker Desktop
* OWASP Juice Shop
* OWASP ZAP
* Web Browser
* Browser Developer Tools

### Security References

* OWASP Top 10
* Common Weakness Enumeration (CWE)
* OWASP Security Guidance

---

## 🔍 5. Assessment Methodology

The assessment follows a structured security testing workflow:

1. Deploy the OWASP Juice Shop application
2. Verify application availability
3. Perform manual application reconnaissance
4. Explore application functionality and user inputs
5. Review authentication and session behaviour
6. Inspect HTTP requests and responses
7. Perform manual security testing
8. Configure and run an OWASP ZAP automated scan
9. Review and validate ZAP alerts
10. Classify identified vulnerabilities
11. Document findings and supporting evidence
12. Analyze potential security impact
13. Provide remediation recommendations

---

## 🚀 6. Lab Setup

### Prerequisites

The following components were used for the assessment:

* Docker Desktop
* OWASP ZAP
* Web Browser

### Deploy OWASP Juice Shop

Pull the official Juice Shop Docker image:

```bash
docker pull bkimminich/juice-shop
```

Run the application:

```bash
docker run -d -p 3000:3000 bkimminich/juice-shop
```

### Verify the Container

```bash
docker ps
```

The Juice Shop application should be accessible through:

```text
http://localhost:3000
```

---

# 🕷️ 7. OWASP ZAP Security Scanning

OWASP ZAP was used to perform automated web application security testing.

### ZAP Configuration

The following steps were performed:

1. Launch OWASP ZAP
2. Configure the Juice Shop target
3. Start the automated scan
4. Allow ZAP to crawl and analyze the application
5. Review passive scan results
6. Review active scan results where applicable
7. Analyze generated alerts
8. Validate relevant findings
9. Generate the ZAP security report

### Scan Results

The generated ZAP results were reviewed to identify potential:

* Security misconfigurations
* Missing security headers
* Injection-related issues
* Authentication/session-related weaknesses
* Information disclosure
* Cross-origin configuration issues
* Other web application security weaknesses

Only relevant and validated findings should be included in the final vulnerability assessment.

---

# 📁 8. Repository Structure

```text
Vortextech-cybersec-week3/
│
├── README.md
├── FINDINGS.md
│
├── Audit Report/
│   ├── README.md
│   └── VortexTech-Week3-Audit-Report.pdf
│
├── Evidence/
│   ├── 1) Setting up and Running Docker.png
│   ├── 2) Running ZAP Automated Scan.png
│   └── 3) Automated Scan Results.png
│
└── zap-reports/
    └── ZAP-Scan-Report.pdf
```

---

# 🛡️ 9. Remediation Summary

Based on the identified vulnerabilities, recommended security improvements may include:

* Use parameterized queries to prevent injection attacks
* Implement an appropriate **Content Security Policy (CSP)**
* Review and restrict **CORS** policies
* Implement appropriate anti-clickjacking protections
* Avoid exposing session identifiers through URLs
* Remove unnecessary internal or sensitive information from responses
* Implement and properly configure security-related HTTP headers
* Apply least-privilege principles
* Regularly perform vulnerability assessments and security testing
* Maintain secure application configuration and dependencies

Specific remediation recommendations should be linked to the individual findings documented in `FINDINGS.md`.

---

# 📝 10. Conclusion

This assessment evaluated the security of the locally deployed OWASP Juice Shop application using a combination of manual testing and automated scanning with OWASP ZAP.

The assessment identified multiple security weaknesses that were analyzed based on their potential impact and relevant security classifications. Evidence was collected to support the documented findings, and remediation recommendations were provided to demonstrate how the identified issues could be addressed.

The assessment was conducted exclusively within an authorized local practice environment using the intentionally vulnerable OWASP Juice Shop application.

---

# ⚠️ 11. Disclaimer

This security assessment was performed exclusively against the **OWASP Juice Shop intentionally vulnerable practice application** deployed in a local and authorized laboratory environment.

No unauthorized systems, applications, networks, or third-party infrastructure were targeted during this assessment.
