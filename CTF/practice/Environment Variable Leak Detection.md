# 🔐 Environment Variable Leak Detection Challenge

## 📌 Overview
In this challenge, I assumed the role of a junior security analyst who had gained initial shell access to a target system during a security assessment. The objective was to investigate whether sensitive information—such as secret flags or credentials—had been improperly stored in environment variables.

Misconfigured environment variables are a common real-world vulnerability, often leading to credential leakage, privilege escalation, or lateral movement. This exercise simulated how attackers and defenders alike inspect runtime environments for exposed secrets.

---

## 🎯 Objectives
- Enumerate all environment variables on the system
- Identify variables containing sensitive or secret values
- Detect secrets exposed across different shell environments
- Retrieve and confirm the presence of hidden flags

---

## 🧠 Skills Demonstrated
- Linux environment variable inspection
- Secure shell awareness (bash vs zsh)
- Sensitive data discovery
- Command-line filtering and pattern matching
- Security misconfiguration analysis

---

## 🛠️ Tools Used
- Linux shell
- `env`
- `printenv`
- `grep` (extended regular expressions)
- Shell switching (`bash`, `zsh`)

---

## 🧪 Methodology

1. Enumerated all exported environment variables.
2. Filtered variables using pattern matching to identify secrets.
3. Searched for multiple keyword patterns simultaneously.
4. Switched between different shell environments to uncover additional leaks.
5. Retrieved and confirmed all exposed secret values.

---

## ✅ Used Command

```bash
# List all environment variables
env

# Filter for potential secrets
env | grep -Ei "flag|secret|hidden|key"

# Use extended regex for multiple patterns
env | grep -E "FLAG|SECRET"

# Switch shells to inspect different environments
bash
env | grep -Ei "flag|secret|hidden"

```
## 🏁 Result
All secret flags stored in environment variables were successfully identified, confirming a critical security misconfiguration on the system.

