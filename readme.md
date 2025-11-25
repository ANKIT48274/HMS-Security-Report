### **Hospital Management System (HMS) – Critical Security Vulnerabilities Identified**

**Researcher:** Ankit Patidar
**Role:** Ethical Security Researcher

---

## 🚨 Overview

During a security assessment of the open-source **Hospital Management System (HMS)** project by PHPGurukul, multiple **critical-level vulnerabilities** were discovered. These vulnerabilities allow:

* Full database extraction
* Complete admin login bypass
* Reflected XSS execution
* Exposure of sensitive data

All findings have been responsibly disclosed to the vendor.

---

## 🔥 1. SQL Injection (Critical – Full Database Extraction)

**Vulnerable File:**

```
/hospital/hms/admin/index.php
```

**Vulnerable Parameter:**

```
POST → username
```

Using SQLMap, full database extraction was possible.

### ✔ Extracted Database Info:

```
Database: hms
Table: admin
username: admin
password_hash: e10adc3949ba59abbe56e057f20f883e
cracked password: 123456
```

### ✔ SQLMap Proof:

* Boolean-based blind
* Error-based
* Time-based blind
* UNION query tests
  All confirmed injectable.

---

## 🔓 2. Authentication Bypass (Admin Login Bypass)

The login system does NOT properly sanitize input.

**Working Payload:**

```
admin' OR '1'='1' -- 
```

This payload bypasses authentication and gives **full admin dashboard access**.

---

## 🧨 3. Reflected XSS Vulnerability

Multiple input fields directly reflect user input without sanitization.

**Trigger Payload:**

```html
"><script>alert(1)</script>
```

This opens a JavaScript alert box and can be used for:

* Session Hijacking
* Cookie Theft
* Admin Takeover

---

## 📸 Screenshots & Evidence

All PoCs include:

* SQLMap terminal logs
* Dashboard access after bypass
* XSS popup execution
* Raw vulnerable queries shown by server

*(Screenshots attached separately in repo — you can upload your images folder here)*

---

## 🛡 Recommended Fixes

### ✔ SQL Injection Fix

* Use **prepared statements / parameterized queries**
* Avoid directly concatenating user input

### ✔ XSS Fix

* Escape output using `htmlspecialchars()`
* Add server-side validation
* Implement Content Security Policy (CSP)

### ✔ Authentication Fix

* Strict input validation
* Remove inline SQL
* Implement secure login validations

---

## 🤝 Responsible Disclosure

These findings were reported in **good faith** to PHPGurukul via email:

📧 **[info@phpgurukul.com](mailto:info@phpgurukul.com)**

Awaiting their acknowledgment and patch updates.

---

## 🙋‍♂️ Author

**Ankit Patidar**
Ethical Security Researcher
Bug Bounty & Penetration Testing Enthusiast
