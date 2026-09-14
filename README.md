# Mediroza General Hospital — Web Application Penetration Test

![Status](https://img.shields.io/badge/Project-Completed-success)
![Assessment](https://img.shields.io/badge/Assessment-Web%20Application%20Security-blue)
![Methodology](https://img.shields.io/badge/Methodology-Black--Box-orange)
![Severity](https://img.shields.io/badge/Overall%20Risk-Critical-red)

## 📌 Project Overview

This project was completed as part of the **Networkwalks Cybersecurity Program — Batch B082, Week 4 Capstone Project**.

The assessment involved an **authorized black-box penetration test** of the Mediroza General Hospital web application to identify security weaknesses, demonstrate their potential impact through controlled exploitation, and provide practical remediation recommendations.

The assessment demonstrated a complete attack chain beginning with web reconnaissance and authentication testing and progressing through SQL injection, unauthorized access to protected documents, weak document passwords, metadata exposure, directory listing, and exposure of an unsecured database backup.

> **⚠️ Responsible Disclosure Notice**
>
> This repository is intentionally sanitized. No patient records, employee personal information, salaries, shareholder information, credentials, database dumps, confidential documents, or sensitive exploitation evidence are included.
>
> The original penetration-testing report was classified as confidential and is not published in this repository.

---

## 🎯 Objectives

The primary objectives of the assessment were to:

* Evaluate the security posture of the target web application.
* Identify authentication and input-validation weaknesses.
* Test for SQL injection vulnerabilities.
* Assess access controls protecting sensitive documents.
* Evaluate the strength of PDF document protection.
* Inspect document metadata for unintended information disclosure.
* Identify publicly accessible directories and files.
* Assess the security implications of exposed database backups.
* Demonstrate the impact of identified vulnerabilities.
* Provide actionable remediation recommendations.

---

## 🧪 Assessment Methodology

The assessment followed a structured black-box penetration-testing methodology consisting of four primary phases:

### 1. Reconnaissance

Passive and application-level reconnaissance was performed to identify:

* Publicly accessible resources
* Application entry points
* Authentication mechanisms
* Potentially hidden directories
* Information disclosed through web configuration files
* Application behaviour

### 2. Vulnerability Identification

The application was tested for weaknesses including:

* Username enumeration
* Authentication weaknesses
* SQL injection
* Broken access control
* Sensitive information exposure
* Weak document protection
* Metadata leakage
* Directory listing
* Insecure backup storage

### 3. Controlled Exploitation

Identified vulnerabilities were validated through controlled exploitation to determine their actual security impact.

The assessment demonstrated how multiple individually exploitable weaknesses could be chained together to obtain progressively more sensitive information.

### 4. Documentation & Remediation

Each finding was documented with:

* Vulnerability description
* Affected location
* Severity
* Testing methodology
* Security impact
* Evidence
* Recommended remediation

---

# 🛠️ Tools Used

| Tool                              | Purpose                                                                          |
| --------------------------------- | -------------------------------------------------------------------------------- |
| **Burp Suite**                    | HTTP interception, request manipulation and authentication testing               |
| **Firefox**                       | Web application testing and browser-based analysis                               |
| **Browser Developer Tools**       | Source-code and client-side behaviour inspection                                 |
| **cURL**                          | HTTP requests and server-response analysis                                       |
| **wget**                          | Controlled retrieval of publicly accessible resources                            |
| **ExifTool**                      | PDF metadata inspection                                                          |
| **qpdf**                          | PDF security/decryption analysis                                                 |
| **Networkwalks Hash Calculator**  | Extraction of crackable PDF password hashes                                      |
| **Networkwalks Password Cracker** | Password-strength testing using wordlists                                        |
| **ChatGPT**                       | Transformation of raw SQL records into readable, non-sensitive analytical tables |

---

# 🔎 Key Findings

The assessment identified **seven security findings**, ranging from **Medium to Critical** severity.

| # | Finding                                            | Severity    |
| - | -------------------------------------------------- | ----------- |
| 1 | Username Enumeration                               | 🟠 Medium   |
| 2 | SQL Injection Authentication Bypass                | 🔴 Critical |
| 3 | Unauthorized Access to Protected Patient Documents | 🔴 High     |
| 4 | Weak PDF Passwords                                 | 🔴 High     |
| 5 | Sensitive PDF Metadata Exposure                    | 🟠 Medium   |
| 6 | Publicly Accessible Backup Directory               | 🔴 Critical |
| 7 | Confidential Data Exposure Through Database Backup | 🔴 Critical |

---

# 🔴 Critical Finding 1 — SQL Injection Authentication Bypass

### Description

The patient authentication functionality was found to be vulnerable to SQL injection.

Application input was incorporated into a database query without adequate parameterization, allowing specially crafted input to alter the intended SQL query logic.

### Impact

Successful exploitation allowed authentication controls to be bypassed without knowledge of the legitimate account password.

This represented a **Critical** security issue because authentication bypass provided access to functionality intended for authenticated users.

### Security Risk

An attacker exploiting this vulnerability could potentially:

* Bypass authentication
* Access restricted application functionality
* Access information belonging to other users
* Continue further attacks from an authenticated context

### Recommended Remediation

Implement **parameterized queries / prepared statements** and never construct SQL queries directly from untrusted user input.

Additional controls should include:

* Server-side input validation
* Secure password hashing
* Generic authentication error messages
* Least-privilege database accounts
* Centralized logging and monitoring

---

# 🔴 Critical Finding 2 — Publicly Accessible Backup Directory

### Description

A legacy backup directory was publicly accessible through the web server and allowed directory listing.

The directory exposed a database backup file that should never have been available through the public web root.

### Impact

An unauthenticated visitor could discover and retrieve a database backup containing sensitive organizational information.

This significantly increased the impact of the other vulnerabilities identified during the assessment.

### Recommended Remediation

* Remove database backups from the web root.
* Store backups outside publicly accessible directories.
* Disable directory listing.
* Apply appropriate filesystem permissions.
* Implement access controls around backup storage.
* Regularly review legacy and forgotten directories.
* Monitor publicly exposed files using automated security checks.

---

# 🔴 Critical Finding 3 — Sensitive Data Exposure

The exposed database backup contained highly sensitive organizational information.

The assessment confirmed exposure of categories of information including:

* Employee names
* Job titles
* Departments
* Salary information
* Contact information
* National identification information
* Shareholder names
* Shareholding percentages
* Share classes

### Impact

Exposure of this information could result in:

* Privacy violations
* Identity theft
* Financial fraud
* Social engineering
* Employee targeting
* Corporate intelligence gathering
* Regulatory and legal consequences
* Reputational damage

> **Note:** Actual personal records and confidential database contents have deliberately been excluded from this repository.

---

# 🔴 High Finding — Unauthorized Access to Patient Documents

Following successful authentication bypass testing, protected patient laboratory documents were accessible through the application.

The documents contained sensitive medical information and should have been restricted to appropriately authorized users.

### Impact

Unauthorized access to medical documents could result in:

* Patient privacy violations
* Medical confidentiality breaches
* Regulatory consequences
* Targeted social engineering
* Reputational damage

### Recommended Remediation

Sensitive documents should:

* Be stored outside the public web root.
* Require server-side authorization checks.
* Use object-level access controls.
* Verify that the requesting user is authorized to access the specific document.
* Avoid predictable document paths.
* Log document access attempts.

---

# 🔴 High Finding — Weak PDF Password Protection

The assessment demonstrated that the passwords protecting the patient documents were weak enough to be recovered using dictionary-based password attacks.

### Impact

Password-protected files should not be considered secure when predictable or commonly used passwords are selected.

### Recommended Remediation

Organizations should:

* Use strong, randomly generated passwords.
* Avoid dictionary-based passwords.
* Enforce appropriate password complexity.
* Consider modern encryption mechanisms for highly sensitive documents.
* Avoid relying on document passwords as the primary access-control mechanism.

Most importantly, **document encryption should supplement proper server-side authorization rather than replace it.**

---

# 🟠 Medium Finding — Username Enumeration

The login functionality returned distinguishable responses depending on whether an account existed.

This allowed an attacker to determine potentially valid usernames.

### Impact

Username enumeration can assist attackers in:

* Identifying valid accounts
* Performing credential attacks
* Conducting targeted phishing
* Building user/account lists

### Recommended Remediation

Use a consistent authentication failure response regardless of whether:

* The username is invalid
* The password is invalid
* The account does not exist

For example:

> `Invalid credentials. Please try again.`

---

# 🟠 Medium Finding — Sensitive PDF Metadata

One of the documents contained internal metadata that revealed information about the organization's internal environment.

Metadata can unintentionally disclose:

* Employee usernames
* Author names
* Internal comments
* File paths
* Application names
* Creation information
* Internal operational details

### Recommended Remediation

Before sensitive documents are distributed externally:

* Remove unnecessary metadata.
* Establish document sanitization procedures.
* Review automated document-generation systems.
* Prevent internal comments from being embedded in external documents.

---

# 🔗 Attack Chain

One of the most important lessons from this assessment was that the vulnerabilities did not exist in isolation.

The findings formed a chained attack path:

```text
Public Reconnaissance
        │
        ▼
Information Disclosure
        │
        ▼
Username Enumeration
        │
        ▼
SQL Injection
        │
        ▼
Authentication Bypass
        │
        ▼
Protected Patient Documents
        │
        ▼
Weak Document Passwords
        │
        ▼
Sensitive PDF Metadata
        │
        ▼
Discovery of Legacy Backup Location
        │
        ▼
Public Directory Listing
        │
        ▼
Database Backup Exposure
        │
        ▼
Confidential Organizational Data
```

This demonstrates why cybersecurity assessments should evaluate not only individual vulnerabilities but also how weaknesses can be **chained together to produce a much larger security impact**.

---

# 📊 Risk Assessment

| Severity    | Number of Findings |
| ----------- | -----------------: |
| 🔴 Critical |                  3 |
| 🔴 High     |                  2 |
| 🟠 Medium   |                  2 |
| 🟢 Low      |                  0 |

### Overall Risk Rating

# 🔴 CRITICAL

The overall security posture was assessed as **Critical** because several vulnerabilities could be exploited without sophisticated techniques and could ultimately result in unauthorized access to sensitive patient and organizational information.

---

# 🛡️ Remediation Priorities

## Priority 1 — Immediate

### Fix SQL Injection

Implement prepared statements / parameterized queries across the application.

### Remove Public Database Backups

Immediately remove exposed database backups from the web-accessible directory.

### Protect Sensitive Data

Review all publicly accessible files and remove confidential information from the web root.

---

## Priority 2 — High

### Implement Proper Authorization

Ensure authenticated users can only access resources they are explicitly authorized to access.

### Secure Patient Documents

Move sensitive documents outside the public web root and serve them through an authorization-controlled backend.

### Strengthen Document Security

Use strong, unique encryption credentials where document encryption is required.

---

## Priority 3 — Medium

### Prevent Username Enumeration

Return consistent authentication error messages.

### Sanitize Document Metadata

Remove unnecessary internal metadata before documents are distributed.

### Disable Directory Listing

Disable directory indexing across all production directories.

---

# 🧠 Key Lessons Learned

This project provided practical experience in:

* Web application reconnaissance
* Black-box penetration testing
* Authentication security testing
* SQL injection identification
* Authentication bypass
* Access-control testing
* Sensitive information disclosure
* PDF security analysis
* Metadata analysis
* Directory enumeration
* Backup security assessment
* Attack-chain development
* Risk classification
* Security reporting
* Remediation planning

A major lesson from the assessment was that **a secure application requires multiple layers of defense**.

For example, even if a login vulnerability exists, strong authorization controls should prevent unauthorized users from accessing another patient's records. Similarly, sensitive database backups should never be publicly accessible even if other application controls fail.

---

# 📁 Recommended Repository Structure

```text
Mediroza-Web-Application-Penetration-Test/
│
├── README.md
│
├── report/
│   └── sanitized-pentest-report.pdf
│
├── screenshots/
│   ├── reconnaissance/
│   ├── authentication/
│   ├── vulnerability-findings/
│   └── remediation/
│
├── evidence/
│   └── sanitized-evidence/
│
└── LICENSE
```

### Important

Do **not** commit the following to a public repository:

```text
patient_report_1.pdf
patient_report_2.pdf
patient_report_3.pdf
*.sql
database backups
employee salary records
patient information
National ID numbers
phone numbers
email addresses
passwords
password hashes
credentials
session tokens
private reports
confidential screenshots
```

Use **redacted screenshots** or recreated demonstrations where necessary.

---

# 📝 Professional Report

The complete penetration-testing report was submitted as part of the Networkwalks B082 Week 4 Capstone Project.

Because the original report contains confidential information relating to the assessed organization and sensitive data discovered during testing, the original report is **not publicly distributed through this repository**.

A sanitized version may be included where permitted.

---

# 👨‍💻 Author

**Olawale Sogbesan**

Cybersecurity Practitioner | Web Application Security | Penetration Testing

### Program

**Networkwalks Cybersecurity Program**

**Batch:** B082
**Project:** Week 4 Capstone
**Mentor:** Waqas Karim, CCIE

---

# ⚖️ Disclaimer

This project was conducted as an **authorized security assessment within the defined scope of the capstone exercise**.

The techniques and tools documented here are intended for authorized security testing, cybersecurity education, and defensive security research.

Do not perform penetration testing against systems that you do not own or do not have explicit authorization to assess.

Sensitive information discovered during the assessment has been intentionally excluded from this public repository.

---

# ⭐ Skills Demonstrated

`Web Application Security` · `Penetration Testing` · `SQL Injection` · `Authentication Testing` · `Access Control` · `Information Disclosure` · `Reconnaissance` · `Burp Suite` · `cURL` · `ExifTool` · `qpdf` · `PDF Security` · `Risk Assessment` · `Security Reporting` · `Remediation`

---

## Project Status

**Completed — Networkwalks B082 Week 4 Capstone Project**

**Overall Assessment Rating: 🔴 Critical**
