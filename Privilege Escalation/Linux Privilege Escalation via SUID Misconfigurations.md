# 🔓 Linux Privilege Escalation via SUID Misconfigurations

## 📌 Overview
In this lab, I explored **SUID (Set User ID)** permissions on Linux systems and demonstrated how **misconfigured SUID binaries** can be exploited to escalate privileges to **root**.

SUID binaries execute with the **permissions of the file owner**, not the user running them. While this is sometimes required for legitimate administrative tasks, it becomes a **critical security risk** when applied to powerful utilities such as `bash`, `find`, `cp`, or `mv`.

This lab simulates realistic post-exploitation scenarios commonly encountered during penetration tests and internal security audits.

---

## 🎯 Objectives
- Understand how SUID permissions work in Linux
- Identify SUID-enabled binaries
- Exploit unsafe SUID binaries to gain elevated privileges
- Practice multiple privilege escalation techniques
- Learn defensive detection and mitigation strategies

---

## 🧠 Skills Demonstrated
- Linux permission model (`rws`, numeric permissions)
- SUID risk assessment
- Local privilege escalation techniques
- Abuse of trusted binaries
- Secure system hardening concepts
- Post-exploitation methodology

---

## 🛠️ Tools & Commands Used
- Core Linux utilities: `ls`, `find`, `whoami`, `id`, `su`
- SUID exploitation techniques
- File system inspection and analysis

---

## 🔍 Understanding SUID
When a binary has the **SUID bit** set:
- It runs with the **effective UID of the file owner**
- If owned by `root`, the program executes with root privileges

Example permission output:
```bash
-rwsr-xr-x 1 root root /path/to/binary

```

## 🧪 Exploitation Techniques
### 1️⃣ Exploiting SUID bash
If bash has SUID enabled, it can spawn a root shell directly.
Impact: Immediate root access
 Risk Level: Critical
Key concept:
bash -p preserves elevated privileges



### 2️⃣ Exploiting SUID find
find can execute arbitrary commands using -exec.
Impact: Root shell execution
 Risk Level: High
Key concept:
Abuse -exec to run /bin/bash with preserved privileges



### 3️⃣ Exploiting SUID cp and mv
If file manipulation utilities have SUID enabled, attackers can:
Overwrite protected system files


Modify authentication databases


Create or escalate privileged accounts


Impact: Persistent root access
 Risk Level: Critical
Key concept:
Overwriting /etc/passwd or similar sensitive files



## 🔎 Discovering SUID Binaries
During reconnaissance, SUID binaries were enumerated using standard audit techniques:
```bash
find / -user root -perm -4000 -type f 2>/dev/null
```

This identifies root-owned executables with SUID enabled, which require manual review to determine exploitability.

## 🚨 Security Impact
Improper SUID usage can lead to:
Full system compromise


Privilege escalation without credentials


Persistence mechanisms for attackers


Bypass of authentication controls


SUID misconfigurations are frequently exploited in:
Capture-the-Flag environments


Real-world Linux breaches


Internal penetration tests



## 🛡️ Defensive Recommendations
Avoid SUID on shells and file manipulation tools


Restrict SUID usage to strictly required binaries


Audit regularly using automated scanners


Apply the principle of least privilege


Monitor filesystem permission changes


Use security baselines (CIS Benchmarks)



## 🏁 Key Takeaways
SUID is powerful but dangerous


Even trusted binaries become attack vectors when misconfigured


Enumeration + creativity = privilege escalation


Proper permission hygiene is critical for Linux security

