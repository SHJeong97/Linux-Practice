# 🧨 Linux Privilege Escalation via Tar Wildcard Injection (Cron Abuse)

## 📌 Overview
In this lab, I demonstrated **wildcard injection**, a Linux privilege escalation technique that abuses how shell wildcards (`*`) are expanded when used insecurely inside **cron-executed commands**.

The lab focuses on exploiting the `tar` command when it is executed by a **root-owned cron job** using a wildcard (`*`) inside a **writable directory**. By crafting specially named files, attacker-controlled input is interpreted as **tar command-line options**, resulting in **arbitrary command execution as root**.

This vulnerability represents a realistic post-exploitation scenario frequently observed during internal penetration tests and misconfigured server environments.

---

## 🎯 Objectives
- Understand shell wildcard expansion
- Identify insecure cron job configurations
- Exploit tar option injection via crafted filenames
- Achieve root privilege escalation through scheduled task abuse
- Validate escalation by writing to `/root`

---

## 🧠 Skills Demonstrated
- Linux privilege escalation
- Cron job analysis
- Shell wildcard behavior
- Command-line option injection
- Abuse of automation mechanisms
- Reverse shell execution
- Linux file permission analysis

---

## 🛠️ Tools & Commands Used
- `cron`, `crontab`
- `tar`
- `nc.traditional`
- `ls`, `cat`, `echo`, `touch`
- Shell wildcard expansion (`*`)

---

## 🔍 Vulnerability Breakdown

### Misconfigured Cron Job
A root-owned cron job executed the following command every minute:

```bash
cd /var/www/html/ && tar -zcf /var/backups/html.tgz *

```

Why this is dangerous:
* expands to all filenames


Filenames starting with -- are treated as tar options


Directory was world-writable


Command executed as root



## 🧪 Exploitation Technique
Wildcard Injection via Tar Options
By creating files with names such as:
--checkpoint=1


--checkpoint-action=exec=sh shell.sh


The wildcard expansion caused tar to interpret filenames as command-line flags, resulting in execution of an attacker-controlled script.
Because the cron job ran as root, the injected command executed with full root privileges.

## 🧬 MITRE ATT&CK Mapping
| Tactic                             | Technique ID | Technique Name                        | Description                                                |
| ---------------------------------- | ------------ | ------------------------------------- | ---------------------------------------------------------- |
| Privilege Escalation               | T1053.003    | Scheduled Task / Cron                 | Abuse of cron jobs to execute attacker-controlled commands |
| Execution                          | T1059        | Command and Scripting Interpreter     | Execution of shell commands via injected tar options       |
| Privilege Escalation               | T1068        | Exploitation for Privilege Escalation | Exploiting misconfigurations to gain elevated privileges   |
| Persistence                        | T1053.003    | Scheduled Task / Cron                 | Repeated execution through cron scheduling                 |
| Defense Evasion                    | T1070        | Indicator Removal on Host             | Non-interactive execution reduces audit visibility         |
| Initial Access (Post-Exploitation) | T1190        | Exploit Public-Facing Application     | Writable web directories enabling injection                |



## 🚨 Security Impact
Successful exploitation results in:
Silent root command execution


Privilege escalation without authentication


Automation-based persistence


Full system compromise


This attack is especially dangerous because:
It requires no memory corruption


It relies on legitimate system utilities


Execution occurs automatically


Logs are often minimal or overlooked



## 🛡️ Defensive Recommendations
Avoid using wildcards in cron jobs


Use -- to explicitly terminate option parsing:

 tar -zcf backup.tgz -- *


Use absolute paths (./*) to prevent option injection


Restrict write permissions on directories used by cron


Run cron jobs with least privilege


Monitor cron execution and file integrity



## 🏁 Key Takeaways
Wildcard expansion is a powerful but dangerous feature


Tar option injection is a known, exploitable behavior


Cron jobs are high-value privilege escalation targets


File permissions matter as much as command syntax

