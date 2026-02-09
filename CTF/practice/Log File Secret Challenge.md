# 🔍 Log File Secret Challenge — Linux Log Analysis

## 📌 Overview
In this challenge, I acted as a Junior System Administrator tasked with locating a hidden flag embedded within system log files. The exercise simulates a real-world operational scenario where administrators must search logs to diagnose issues, investigate incidents, or uncover hidden indicators.

The goal was to efficiently navigate the Linux filesystem and search through log data using command-line tools—core skills for system administration, incident response, and security operations.

---

## 🎯 Objectives
- Navigate the Linux filesystem to access system logs
- Search recursively through log files for specific indicators
- Identify and extract a flag formatted as `FLAG{...}`
- Demonstrate effective use of Linux text-processing tools

---

## 🧠 Skills Demonstrated
- Linux filesystem navigation
- Log file analysis
- Recursive text searching
- Command-line efficiency
- Incident investigation fundamentals

---

## 🛠️ Tools Used
- Linux shell
- `cd` (directory navigation)
- `ls` (file inspection)
- `grep` (recursive text search)

---

## 🧪 Methodology

1. Navigated to the system log directory.
2. Enumerated available log files.
3. Performed a recursive search for the flag pattern.
4. Identified the log entry containing the flag.
5. Displayed the full flag content.

---

## ✅ Used Command

```bash
# Navigate to the system log directory
cd /var/log

# List available log files and directories
ls

# Recursively search for lines containing the flag pattern
grep -r "FLAG{" .

```

##  🏁 Result
The hidden flag was successfully located within the system logs, confirming the ability to efficiently search and analyze log files using command-line tools.

