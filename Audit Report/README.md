# OWASP Juice Shop Security Audit

A structured security audit of the OWASP Juice Shop deliberately-vulnerable web application using automated scanning with OWASP ZAP.

## What This Project Does

This repository contains a complete security audit of OWASP Juice Shop, including:
- Automated vulnerability scanning using OWASP ZAP v2.17.0
- Documentation of 3+ security findings with real-world impact
- Remediation recommendations for each vulnerability

## Setup & How to Run

### Prerequisites
- Docker Desktop installed
- OWASP ZAP installed (from zaproxy.org)

### Run Juice Shop
```bash
docker pull bkimminich/juice-shop
docker run -d -p 3000:3000 bkimminich/juice-shop
```
Access at: `http://localhost:3000`

### Run ZAP Scan
1. Open OWASP ZAP
2. Go to: Tools → Automated Scan
3. Enter URL: `http://localhost:3000`
4. Click: Start Scan
5. Once complete, generate report and save

## Findings

This audit identified **9 vulnerabilities** across multiple risk categories. See [`FINDINGS.md`](./FINDINGS.md) for detailed analysis of the top 3 findings:

1. **SQL Injection** (High Risk)
2. **CSP Header Not Set** (Medium Risk)
3. **Private IP Disclosure** (Low Risk)

Each finding includes steps to reproduce, real-world impact, and practical remediation code.

## Files

- `README.md` — This file
- `FINDINGS.md` — Detailed documentation of all findings
- `zap-reports/` — Raw ZAP scan output
- `Evidence/` — Contains Visuals Of the Setup, Automated Scan, Results 

## Author

Hadi Faheem

---

**Disclaimer:** This audit was conducted on OWASP Juice Shop, a deliberately vulnerable practice application designed for security training. No unauthorized systems were tested.


