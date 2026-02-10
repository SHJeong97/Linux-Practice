# 🔐 Linux Privilege Escalation via /etc/passwd Misconfiguration

## 📌 Overview
In this lab, I explored how misconfigurations in Linux authentication files—specifically `/etc/passwd`—can lead to **privilege escalation**. These files play a critical role in user authentication and authorization, and improper permissions or insecure storage of password hashes can allow attackers to gain full administrative access.

This exercise simulated real-world post-exploitation scenarios commonly encountered during penetration tests and security audits.

---

## 🎯 Objectives
- Understand the structure and purpose of `/etc/passwd` and `/etc/shadow`
- Identify dangerous permission misconfigurations
- Exploit writable authentication files for privilege escalation
- Perform password hash extraction and cracking
- Escalate from a low-privileged user to root access

---

## 🧠 Skills Demonstrated
- Linux authentication internals
- Privilege escalation techniques
- File permission analysis
- Password hash handling
- Post-exploitation methodology
- Secure system hardening awareness

---

## 🛠️ Tools Used
- Linux terminal
- `grep`, `cat`, `ls`, `id`, `whoami`
- `openssl`
- `john` (John the Ripper)
- `su`

---

## 🧪 Scenarios Covered

### 1️⃣ Writable `/etc/passwd` File (Critical Misconfiguration)
A misconfigured system allowed **write access** to `/etc/passwd` for non-root users.

**Attack logic:**
- Extract a valid root user entry format
- Create a new user entry with UID `0`
- Insert a password hash
- Append the entry to `/etc/passwd`
- Switch users and gain root privileges

```bash
# Check permissions
ls -l /etc/passwd /etc/shadow

# Extract root entry format
grep root /etc/passwd > new_user_entry.txt

# Append malicious entry
echo "<crafted_entry>" >> /etc/passwd

# Switch to new privileged user
su <username>
```
### 2️⃣ Password Hash Stored in /etc/passwd
In a second scenario, /etc/passwd had correct permissions, but the root password hash was incorrectly stored in the file instead of /etc/shadow.
Attack logic:
Extract root hash from /etc/passwd


Use offline password cracking


Authenticate as root using cracked credentials
```bash
# Extract root hash
grep ^root /etc/passwd > hash.txt

# Crack hash (dictionary attack)
john hash.txt > cracked.txt

# Switch to root after cracking
su root

```

## 🏁 Result
Successfully escalated privileges to root in two different misconfiguration scenarios, demonstrating how small authentication mistakes can completely compromise system security.

