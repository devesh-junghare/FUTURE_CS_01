# FUTURE_CS_01
# Web Application Security Testing (bWAPP)

## Overview
This repository documents a hands-on web application security assessment conducted on **bWAPP**, a deliberately vulnerable web application. The purpose of this project is to identify common web vulnerabilities, validate findings using automated and manual testing, and provide actionable mitigation strategies.

## Objectives
- Learn and apply web application security testing techniques.
- Identify and confirm vulnerabilities such as SQL Injection, XSS, CSRF, authentication flaws, and insecure cookie handling.
- Produce a detailed security report with evidence and remediation steps.
- Gain practical experience with penetration testing tools including OWASP ZAP, Burp Suite, sqlmap, and Nikto.

## Tools Used
- **OWASP ZAP** — automated scanning and active testing.
- **Burp Suite** — request interception, session testing, and manual verification.
- **sqlmap** — SQL injection testing and verification.
- **Nikto** — basic web server vulnerability scanning.
- **Firefox/Chrome** — browser testing with proxy configured to ZAP/Burp.

## Methodology
1. **Setup & Recon**
   - Deploy bWAPP locally (VM or container) and confirm accessibility.
   - Map functional pages: login, profiles, message boards, forms, and file uploads.

2. **Automated Scanning**
   - Run OWASP ZAP to crawl the site and identify potential vulnerabilities.
   - Export scan results and review alerts in the **Alerts** tab.

3. **Manual Verification**
   - Validate automated findings through request inspection and non-destructive testing.
   - Test login, input fields, cookies, session handling, and file upload endpoints.

4. **Reporting**
   - Document each vulnerability with description, evidence, impact, and mitigation.
   - Prioritize findings by risk rating (High / Medium / Low).

## Example Findings

### 1. Hidden File Disclosure — `/phpinfo.php`
- **Risk Rating:** Medium  
- **Description:** The `phpinfo.php` file exposes sensitive server configuration, PHP version, and environment details. Attackers could leverage this information to identify vulnerabilities or craft targeted attacks.  
- **Evidence:** Screenshot of the vulnerable page showing PHP configuration included (*see screenshots/phpinfo_vuln.png*).  
- **Mitigation:** Remove the file in production, or restrict access via authentication and authorization.

### 2. Missing Anti-Clickjacking Header
- **Risk Rating:** Medium  
- **Description:** Responses from the application lack `X-Frame-Options` or `Content-Security-Policy (frame-ancestors)` headers, leaving it vulnerable to clickjacking attacks.  
- **Evidence:** Flagged by OWASP ZAP; screenshot attached (*see screenshots/clickjacking_alert.png*).  
- **Mitigation:** Add `X-Frame-Options: DENY` or `Content-Security-Policy: frame-ancestors 'none'` headers to all pages.

### 3. Cookie Missing `HttpOnly` Flag
- **Risk Rating:** Medium  
- **Description:** Cookies without the `HttpOnly` flag can be accessed by JavaScript. Malicious scripts could steal session cookies, potentially leading to session hijacking.  
- **Evidence:** ZAP flagged cookies as accessible by JavaScript; screenshot included (*see screenshots/cookie_httponly.png*).  
- **Mitigation:** Set `HttpOnly`, `Secure`, and `SameSite` attributes on all sensitive cookies.

## Quick Start / Reproduce

```bash
# Install ZAP on Kali Linux
sudo apt update && sudo apt install zaproxy -y

# Launch ZAP GUI
zaproxy

# Example Docker-based headless scan
docker run --rm owasp/zap2docker-stable zap-baseline.py -t http://127.0.0.1 -r zap_report.html
