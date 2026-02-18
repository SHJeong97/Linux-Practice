# 🔐 Privilege Escalation via Brute Force Attacks (Linux)

## 📘 Overview
This lab explores **two brute force–based techniques** used to obtain the root user password on a Linux system. The scenario assumes initial low-privileged access (e.g., `www-data`) and demonstrates how weak authentication controls can lead directly to full system compromise.

Two attack paths are covered:
1. Local brute force against the `su` command
2. Network-based brute force against the SSH service

The purpose of this lab is to highlight **password strength, authentication exposure, and defensive misconfigurations**, not to promote brute-force abuse.

---

## 🎯 Learning Objectives
- Understand how brute force attacks target Linux authentication
- Differentiate local vs. network-based brute force techniques
- Identify risks of weak passwords and exposed services
- Demonstrate privilege escalation using valid credentials
- Map attacker behavior to MITRE ATT&CK and NIST CSF

---

## 🧠 Key Concepts Demonstrated
- Authentication attack fundamentals
- Password guessing and credential discovery
- Local privilege escalation via `su`
- Remote access abuse via SSH
- Root access obtained through weak credential hygiene

---

## 🛠️ Tools & Techniques (High-Level)
- Local authentication brute forcing  
- Network service brute forcing  
- Password dictionary usage  
- Credential validation  
- Privileged shell access  

---

## 🔍 Step Summary (Selected)

### 1️⃣ Initial Low-Privilege Access
```bash
whoami
```
Confirmed execution context as a non-privileged service account, simulating access gained through a web-facing application.

### 2️⃣ Local Brute Force Against su
```bash
sucrack -w 20 /tmp/common-wordlists.txt
```
A local brute force attack was performed against the su command to identify the root password using a dictionary-based approach.
Outcome:
 Valid root credentials were recovered, enabling direct authentication as the root user.

### 3️⃣ Root Access Verification (Local)
```bash
su - root
whoami
```
Successful authentication confirmed escalation to root privileges via local password brute forcing.

### 4️⃣ SSH Configuration Validation
```bash
grep -i permitrootlogin /etc/ssh/sshd_config
```
Verified that the SSH service allowed root authentication, enabling a network-based brute force path.

### 5️⃣ Network Brute Force Against SSH
```bash
hydra -l root -P /tmp/common-wordlists.txt -t 64 127.0.0.1 ssh
```
A parallelized brute force attack was executed against the SSH service using a common password list.
Outcome:
 Valid root credentials were identified over the network.

### 6️⃣ Root Access Verification (Remote)
```bash
su - root
whoami

```

Confirmed root-level access after successful SSH credential discovery.

## 🧬 MITRE ATT&CK Mapping
| Tactic               | Technique ID | Technique Name                    | Description                                           |
| -------------------- | ------------ | --------------------------------- | ----------------------------------------------------- |
| Initial Access       | T1078        | Valid Accounts                    | Use of recovered credentials to authenticate as root  |
| Credential Access    | T1110        | Brute Force                       | Password guessing using local and network-based tools |
| Execution            | T1059        | Command and Scripting Interpreter | Shell execution after authentication                  |
| Privilege Escalation | T1078.003    | Valid Accounts: Local Accounts    | Escalation through legitimate root credentials        |
| Lateral Movement     | T1021.004    | Remote Services: SSH              | Abuse of SSH service for remote authentication        |
| Defense Evasion      | T1070        | Indicator Removal on Host         | Minimal artifacts beyond authentication logs          |


## 🧩 NIST Cybersecurity Framework (CSF) Mapping
| NIST Function | Category | Subcategory                      | Description                                        |
| ------------- | -------- | -------------------------------- | -------------------------------------------------- |
| Identify      | ID.RA    | Risk Assessment                  | Weak passwords enabling brute force compromise     |
| Protect       | PR.AC    | Access Control                   | Inadequate password and authentication enforcement |
| Protect       | PR.IP    | Information Protection Processes | Lack of password complexity and account lockout    |
| Detect        | DE.CM    | Security Continuous Monitoring   | Absence of alerts for brute force attempts         |
| Respond       | RS.AN    | Analysis                         | Analysis of compromise through credential abuse    |
| Recover       | RC.IM    | Improvements                     | Strengthening authentication and access controls   |


## 🔐 Security Impact
This lab demonstrates how weak authentication practices can immediately invalidate other security controls:
Low-privilege access escalated directly to root


Password reuse and simplicity enabled compromise


SSH root login expanded attack surface


No exploitation of vulnerabilities was required


Authentication failures alone were sufficient.

## 🛡️ Defensive Recommendations
Enforce strong, unique passwords for all accounts


Disable root login over SSH


Implement account lockout and rate limiting


Monitor authentication logs for brute force activity


Use key-based authentication instead of passwords


Apply the principle of least privilege consistently



## 📌 Key Takeaway
Brute force attacks remain effective not because of advanced tools, but because of weak defensive controls.
When passwords are predictable and services are exposed, attackers do not need exploits—
 valid credentials are enough to become root.

