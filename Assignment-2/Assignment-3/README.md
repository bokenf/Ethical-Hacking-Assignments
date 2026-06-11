# Assignment 3: Vulnerability Exploitation & Mitigation

## Objective
To safely test a web application environment for critical security vulnerabilities, execute a controlled exploitation proof-of-concept (PoC) to validate the business impact, and document code-level mitigation techniques.

---

## 1. Vulnerability Assessment & Environment
* **Target Platform:** Localized Web Application (Vulnerable Training Portal Node)
* **Vulnerability Classification:** OWASP Top 10 - Injection (SQL Injection / SQLi)
* **Environment Protocol:** Authorized, sandbox host network segment.

---

## 2. Exploitation Phase & Attack Mechanics

### A. Traffic Interception (Tool: `Burp Suite`)
Web requests targeting the portal's login form authentication system were proxied through Burp Suite. This allowed runtime modification of user string parameters sent via POST request data arrays.

### B. Payload Injection & Authentication Bypass
By inserting structural database control operators into the raw input username field, the backend query logic was successfully manipulated.

* **Injected Security Payload:** `' OR 1=1 -- `
* **Backend Database Evaluation (Vulnerable Logic):**
  ```sql
  SELECT * FROM users WHERE username = '' OR 1=1 --' AND password = 'md5(password)';
