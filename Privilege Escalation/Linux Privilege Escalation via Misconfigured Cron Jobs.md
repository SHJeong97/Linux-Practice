# ⏱️ Linux Privilege Escalation via Misconfigured Cron Jobs

## 📌 Overview
In this lab, I analyzed Linux cron jobs and demonstrated how **misconfigured scheduled tasks** can be exploited to achieve **privilege escalation**.

Cron jobs frequently run with elevated privileges (often as `root`). When scripts executed by cron are **world-writable or improperly secured**, attackers can modify them to execute arbitrary commands with **root-level access**.

This lab simulates a real-world post-exploitation scenario commonly observed during internal penetration tests and red team engagements.

---

## 🎯 Objectives
- Understand cron job scheduling and syntax
- Identify insecure cron configurations
- Analyze cron-executed scripts for permission flaws
- Exploit writable cron scripts for privilege escalation
- Achieve root-level access via scheduled task abuse

---

## 🧠 Skills Demonstrated
- Linux task scheduling (`cron`, `crontab`)
- Cron syntax interpretation
- File permission auditing
- Privilege escalation techniques
- Reverse shell execution concepts
- Abuse of automation mechanisms
- Secure configuration assessment

---

## 🛠️ Tools & Commands Used
- `crontab`, `cron`, `cat`, `ls`, `chmod`
- Script inspection and modification
- Local shell interaction utilities

---

## 🧩 Understanding Cron Jobs
Cron jobs are scheduled tasks defined using the following syntax:

```text
* * * * * [user] command

```
## 🔍 Identifying the Vulnerability
A scheduled task was identified that:
Executed every minute


Ran with root privileges


Referenced a script located in a user-writable directory


This combination represents a critical privilege escalation vulnerability.

## 🧪 Exploitation Techniques
### 1️⃣ Overwriting a Cron-Executed Script
Because the cron-executed script was world-writable, it could be modified by a low-privileged user.
When the cron job executed, attacker-controlled commands ran automatically with root privileges.
Impact: Full privilege escalation
 Severity: Critical

### 2️⃣ Reverse Shell via Cron Execution
The writable cron script was modified to spawn a shell when executed by cron.
Once triggered:
The payload executed automatically


A root-level shell was obtained without authentication


Impact: Remote command execution as root
 Severity: Critical

### 3️⃣ Persistence via SUID Abuse
As an alternative technique, the cron job was abused to:
Modify permissions on system binaries


Set the SUID bit on trusted executables


This allowed persistent root access even after session termination.
Impact: Persistent privilege escalation
 Severity: Critical


## 🧬 MITRE ATT&CK Mapping
| Tactic               | Technique ID | Technique Name                            | Description                                                                 |
| -------------------- | ------------ | ----------------------------------------- | --------------------------------------------------------------------------- |
| Privilege Escalation | T1053.003    | Scheduled Task / Cron                     | Abuse of Linux cron jobs to execute malicious code with elevated privileges |
| Persistence          | T1053.003    | Scheduled Task / Cron                     | Establishing persistence by modifying scheduled tasks                       |
| Execution            | T1059        | Command and Scripting Interpreter         | Execution of attacker-controlled shell commands                             |
| Privilege Escalation | T1548.001    | Abuse Elevation Control Mechanism: Setuid | Leveraging SUID binaries for root access                                    |
| Defense Evasion      | T1070        | Indicator Removal on Host                 | Automated execution reduces interactive artifacts                           |
| Persistence          | T1547        | Boot or Logon Autostart Execution         | Scheduled execution ensures repeated payload execution                      |




## 🚨 Security Impact
Misconfigured cron jobs can lead to:
Silent privilege escalation


Automated execution of attacker-controlled code


Persistent system compromise


Bypass of authentication controls


Cron-based vulnerabilities are especially dangerous because:
Execution is automatic


Minimal logging is generated


They are often overlooked during security audits



## 🛡️ Defensive Recommendations
Avoid running cron jobs as root unless strictly necessary


Ensure cron-executed scripts are owned and writable only by root


Regularly audit /etc/crontab and /etc/cron*


Implement file integrity monitoring (AIDE, auditd)


Enforce the principle of least privilege


Monitor cron execution logs for anomalies



🏁 Key Takeaways
Scheduled tasks are a common and dangerous attack vector


Writable cron scripts guarantee privilege escalation


Automation mechanisms are high-value post-exploitation targets


Proper permission hardening is critical for Linux security



