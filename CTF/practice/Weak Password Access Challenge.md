# 🔐 Weak Password Access Challenge (Linux)

## 📌 Overview
In this challenge, I acted as a junior penetration tester tasked with auditing an internal Linux server for common security weaknesses. During initial assessment, a non-standard user account named `visitor` was identified and suspected to be a misconfigured guest account protected by a weak password.

The goal was to validate this risk by attempting access and locating a honeypot flag file to confirm successful compromise.

---

## 🎯 Objectives
- Identify weak authentication practices
- Gain access to a misconfigured user account
- Locate sensitive files to validate impact
- Demonstrate post-authentication access

---

## 🧠 Skills Demonstrated
- Linux user enumeration
- Privilege context switching
- Hidden file discovery
- Credential hygiene assessment
- Post-exploitation validation

---

## 🛠️ Tools Used
- Linux shell
- `su`
- `ls`
- `cat`

---

## ⚠️ Security Finding
The `visitor` account was protected by a weak, easily guessable password. This allowed unauthorized access without the need for brute-force attacks or advanced tooling.

This type of misconfiguration is a common real-world attack vector.

---

## 🧪 Methodology

1. Switched from the default user to the `visitor` account
2. Navigated to the visitor user’s home directory
3. Enumerated hidden directories
4. Located and read a honeypot flag file
5. Exited the compromised account

---

## ✅ Used Command

```bash
# Switch to the target user account
su visitor

# Navigate to the visitor home directory
cd ~

# List all files including hidden directories
ls -la

# Enter the hidden secure directory
cd .secure

# Display the flag file to confirm access
cat flag.txt

# Exit back to the original user
exit

```
## 🏁 Result
Access to the visitor account was successfully obtained due to weak password configuration. The presence and readability of flag.txt confirmed unauthorized access and demonstrated the potential impact of poor credential management.

