# Security Audit Findings — OWASP Juice Shop

**Scan Date:** Wed, 16 Sept 2026 02:19:42
**Target:** http://localhost:3000 (OWASP Juice Shop)
**Scanner:** OWASP ZAP v2.17.0
**Total Vulnerabilities Found:** 9

---

## Finding 1: SQL Injection (High Risk)

### Summary
SQL injection may be possible on the product search endpoint. An attacker can manipulate SQL queries to bypass authentication, extract sensitive data, or execute arbitrary commands on the database.

### How Found
OWASP ZAP's active scanner tested the `/rest/products/search?q=` parameter with SQL injection payloads:
- Payload tested: `'(`
- Response: HTTP 500 Internal Server Error
- Evidence: Server error behavior indicates SQL injection is possible

### Vulnerability Category
**CWE-89:** Improper Neutralization of Special Elements Used in an SQL Command ('SQL Injection')
**OWASP:** A03:2021 – Injection

### Real-World Impact
- **Authentication Bypass:** Attacker logs in without valid credentials
- **Data Theft:** Extract entire user database, payment info, personal data
- **Data Manipulation:** Modify prices, add admin accounts, alter transactions
- **Remote Code Execution:** Execute OS commands on the server (depending on database configuration)
- **Historical Example:** The Target breach (2013) involved SQL injection and compromised 40 million credit card records

### Remediation

**Use Prepared Statements (Parameterized Queries):**

❌ **Vulnerable Code:**
```javascript
// Node.js - DO NOT DO THIS
const query = `SELECT * FROM users WHERE email = '${userInput}'`;
db.query(query, callback);
```

✅ **Safe Code:**
```javascript
// Node.js - Use parameterized queries
const query = 'SELECT * FROM users WHERE email = ?';
db.query(query, [userInput], callback);
```

**Additional Steps:**
1. Validate input on server side (length, type, format)
2. Use least-privilege database user (no DROP/CREATE permissions)
3. Implement input whitelisting for allowed characters
4. Use ORM frameworks (Sequelize, TypeORM) that handle parameterization automatically
5. Regular SAST/DAST security testing

---

## Finding 2: Content Security Policy (CSP) Header Not Set (Medium Risk)

### Summary
The application does not implement a Content Security Policy HTTP header. This missing security layer leaves the application vulnerable to Cross-Site Scripting (XSS) attacks and data injection attacks.

### How Found
OWASP ZAP's passive scanner analyzed HTTP response headers across all pages:
- **Checked URLs:** http://localhost:3000, http://localhost:3000/, http://localhost:3000/sitemap.xml, etc.
- **Finding:** No `Content-Security-Policy` header present in any response
- **Impact:** Browser has no policy to block malicious inline scripts or external script loading

### Vulnerability Category
**CWE-693:** Protection Mechanism Failure
**OWASP:** A05:2021 – Security Misconfiguration

### Real-World Impact
Without CSP:
- **XSS Exploitation:** Attacker injects malicious JavaScript via reflected/stored XSS
- **Session Hijacking:** Steal cookies and user sessions
- **Credential Theft:** Inject fake login forms to capture passwords
- **Malware Distribution:** Redirect users to malware sites
- **Phishing:** Modify page content to trick users

CSP acts as a "safety net" even if XSS vulnerabilities exist.

### Remediation

**Implement Content-Security-Policy Header:**

✅ **Basic CSP Policy:**
```http
Content-Security-Policy: 
  default-src 'self'; 
  script-src 'self'; 
  style-src 'self' 'unsafe-inline'; 
  img-src 'self' https:; 
  font-src 'self';
```

**Implementation (Express.js):**
```javascript
const express = require('express');
const helmet = require('helmet');
const app = express();

app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"],
    scriptSrc: ["'self'"],
    styleSrc: ["'self'", "'unsafe-inline'"],
    imgSrc: ["'self'", "https:"],
    fontSrc: ["'self'"],
  }
}));

app.listen(3000);
```

**Policy Directives Explained:**
- `default-src 'self'` — Only load content from same origin by default
- `script-src 'self'` — Only allow scripts from same origin (blocks inline scripts)
- `style-src 'self' 'unsafe-inline'` — Allow stylesheets from same origin + inline styles
- `img-src 'self' https:` — Allow images from same origin or any HTTPS source
- `font-src 'self'` — Only load fonts from same origin

**Testing:**
- Use online CSP validators (csp-evaluator.withgoogle.com)
- Monitor browser console for CSP violation reports
- Gradually relax policy only when necessary for features

---

## Finding 3: Private IP Disclosure (Low Risk)

### Summary
Internal private IP addresses are exposed in HTTP response bodies. This information aids attackers in network reconnaissance and internal system targeting.

### How Found
OWASP ZAP's passive scanner searched responses using regex patterns for private IP addresses:
- **Endpoint:** GET `/rest/admin/application-configuration`
- **Evidence Found:**
  - `192.168.99.100:3000`
  - `192.168.99.100:4200`
- **Pattern Matched:** RFC 1918 private IP ranges (10.x.x.x, 172.16.x.x, 192.168.x.x)

### Vulnerability Category
**CWE-497:** Exposure of Sensitive System Information to an Unauthorized Control Sphere
**OWASP:** A01:2021 – Broken Access Control

### Real-World Impact
- **Information Disclosure:** Reveals internal network structure
- **Reconnaissance Aid:** Attackers map internal services and infrastructure
- **Targeted Attacks:** Combined with SSRF vulnerabilities, attackers target specific internal services
- **Compliance Violation:** PCI-DSS, HIPAA, and other standards forbid exposing infrastructure details
- **Attack Surface Mapping:** Part of attacker reconnaissance phase before exploitation

### Remediation

**Remove Private IPs from Responses:**

❌ **Vulnerable Code:**
```javascript
// Node.js - DO NOT DO THIS
app.get('/api/config', (req, res) => {
  res.json({
    database_host: '192.168.1.5',
    cache_server: '10.0.0.3',
    internal_port: 8080
  });
});
```

✅ **Safe Code:**
```javascript
// Node.js - Remove internal details
app.get('/api/config', (req, res) => {
  res.json({
    api_version: '1.0',
    features: ['auth', 'users']
    // No internal IPs, no internal hostnames
  });
});
```

**Error Messages:**

❌ **Vulnerable:**
```javascript
res.status(500).send(error.stack); 
// Stack trace may contain internal IPs
```

✅ **Safe:**
```javascript
res.status(500).send('An error occurred. Please contact support.');
// Log full error server-side only
logger.error(error.stack);
```

**Implementation Steps:**
1. Audit all API responses for private IPs, internal hostnames, paths
2. Use generic error messages in user-facing output
3. Store detailed logs securely (not in web root)
4. Mask sensitive fields in log output
5. Code review: search codebase for hardcoded internal IPs
6. Remove internal documentation from public repositories

---

## Summary Table — All 9 Findings

| # | Finding | Risk | CWE | Action |
|---|---------|------|-----|--------|
| 1 | SQL Injection | 🔴 High | 89 | Detailed above |
| 2 | CSP Header Not Set | 🟠 Medium | 693 | Detailed above |
| 3 | CORS Misconfiguration | 🟠 Medium | 264 | Tighten `Access-Control-Allow-Origin` |
| 4 | Missing Anti-clickjacking Header | 🟠 Medium | 1021 | Add `X-Frame-Options: DENY` |
| 5 | Session ID in URL Rewrite | 🟠 Medium | 598 | Use secure HTTP-only cookies |
| 6 | Private IP Disclosure | 🟡 Low | 497 | Detailed above |
| 7 | Timestamp Disclosure - Unix | 🟡 Low | 497 | Minimize timestamp exposure |
| 8 | X-Content-Type-Options Header Missing | 🟡 Low | 693 | Add `X-Content-Type-Options: nosniff` |
| 9 | Modern Web Application | ℹ️ Info | — | Informational only |

---

## References

- [OWASP Top 10 2021](https://owasp.org/www-project-top-ten/)
- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [OWASP CSP Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)
- [OWASP Juice Shop Documentation](https://owasp-juice.shop/)
- [CWE-89: SQL Injection](https://cwe.mitre.org/data/definitions/89.html)
- [CWE-693: Protection Mechanism Failure](https://cwe.mitre.org/data/definitions/693.html)
- [RFC 1918: Private IP Ranges](https://datatracker.ietf.org/doc/html/rfc1918)
