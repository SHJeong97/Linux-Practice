# 🔐 Privilege Escalation via Sensitive Configuration Files (Linux)

## 📘 Overview
This lab demonstrates how sensitive configuration files and user-related artifacts on a Linux system can be abused to achieve **privilege escalation to root** when proper security controls are not enforced.

The scenario assumes initial low-privileged access obtained through a web-facing service (e.g., `www-data`). From this foothold, filesystem enumeration, credential discovery, and **password reuse** are leveraged to escalate privileges.

The focus of this lab is **credential exposure and operational security weaknesses**, not exploit development.

---

## 🎯 Learning Objectives
- Identify common locations of sensitive configuration files
- Understand risks of plaintext credentials and password reuse
- Recognize how user artifacts leak authentication data
- Demonstrate privilege escalation using valid credentials
- Map attacker behavior to MITRE ATT&CK and NIST CSF

---

## 🧠 Key Concepts Demonstrated
- File and directory enumeration  
- Credential discovery in application configuration files  
- Database access using exposed credentials  
- User shell artifact inspection  
- Privilege escalation via legitimate authentication  

---

## 🛠️ Tools & Techniques (High-Level)
- Linux filesystem enumeration  
- Web application configuration review  
- Database inspection  
- User environment artifact analysis  
- Privileged shell execution  

---

## 🔍 Step Summary  (Selected)

### 1️⃣ Web Configuration File Enumeration
```bash
find /var/www -type f \( -iname "config.php" -o -iname "*.config" \) 2>/dev/null
```

Used to identify web application configuration files that commonly store database credentials. This revealed CMS-related configuration files accessible from the web root.

### 2️⃣ Credential Extraction from Application Configuration
```bash
cat wp-config.php

```

Reviewing the application configuration exposed database authentication details, enabling authenticated access to the backend database service.

### 3️⃣ Database Enumeration for Additional Credentials
```bash
SHOW DATABASES;
SELECT * FROM users;

```

Database access was used to enumerate tables containing stored credentials, revealing additional user accounts and reused passwords.

### 4️⃣ Privilege Escalation via Credential Reuse
```bash
su -

```

Recovered credentials were tested against system accounts. Password reuse enabled successful escalation to a privileged shell.

### 5️⃣ User Artifact Analysis (Shell History)
```bash
cat ~/.bash_history

```

Inspection of user shell history revealed previously executed commands containing authentication material, enabling sudo-level access.

### 6️⃣ Root Access Verification
```bash
whoami

```

Confirmed escalation to root-level privileges after successful authentication.
## 🧬 MITRE ATT&CK Mapping
| Tactic               | Technique ID | Technique Name                              | Description                                             |
| -------------------- | ------------ | ------------------------------------------- | ------------------------------------------------------- |
| Initial Access       | T1190        | Exploit Public-Facing Application           | Initial foothold obtained via a vulnerable web service  |
| Discovery            | T1083        | File and Directory Discovery                | Enumeration of filesystem locations for sensitive files |
| Credential Access    | T1552.001    | Unsecured Credentials: Credentials in Files | Credentials recovered from configuration files          |
| Credential Access    | T1552.003    | Unsecured Credentials: Bash History         | Credentials recovered from user shell history           |
| Execution            | T1059        | Command and Scripting Interpreter           | Execution of shell commands using recovered credentials |
| Privilege Escalation | T1078        | Valid Accounts                              | Escalation using reused legitimate credentials          |
| Defense Evasion      | T1070        | Indicator Removal on Host                   | Limited interactive artifacts during escalation         |
## 🧩 NIST Cybersecurity Framework (CSF) Mapping
| NIST Function | Category | Subcategory                    | Description                                                  |
| ------------- | -------- | ------------------------------ | ------------------------------------------------------------ |
| Identify      | ID.AM    | Asset Management               | Identification of sensitive files and credential locations   |
| Identify      | ID.RA    | Risk Assessment                | Recognition of risk posed by exposed credentials             |
| Protect       | PR.AC    | Access Control                 | Failure to enforce least privilege and credential separation |
| Protect       | PR.DS    | Data Security                  | Insecure storage of authentication data                      |
| Detect        | DE.CM    | Security Continuous Monitoring | Lack of detection for credential misuse                      |
| Respond       | RS.AN    | Analysis                       | Analysis of escalation path using valid credentials          |
| Recover       | RC.IM    | Improvements                   | Lessons learned to improve credential handling               |



## 🔐 Security Impact
This lab illustrates how basic security hygiene failures can undermine layered defenses:
Web application compromise enabled host-level access


Plaintext credentials exposed critical services


Password reuse collapsed privilege boundaries


No advanced exploitation techniques were required.

## 🛡️ Defensive Recommendations
Avoid storing plaintext credentials in application files


Enforce unique credentials per service and account


Restrict permissions on configuration files


Secure or disable shell history for sensitive users


Monitor access to credential-bearing files


Apply least privilege consistently



## 📌 Key Takeaway
Privilege escalation does not always rely on complex exploits.
 In many real environments, credential exposure and reuse are sufficient.
Strong configuration management and credential hygiene remain foundational to Linux security.

