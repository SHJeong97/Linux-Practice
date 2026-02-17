# 🔐 Privilege Escalation via Stepping Stone User (Linux)

## 📘 Overview
This lab demonstrates a **multi-stage privilege escalation** scenario where direct escalation to root is not possible. Instead, escalation is achieved by leveraging an **intermediate “stepping stone” user** with elevated access compared to the initial foothold.

The scenario assumes initial low-privileged access as a web service account (`www-data`). By enumerating system users, permissions, and authentication artifacts, access is first escalated to a regular user (`alice`), and then further escalated to the root user using SUID misconfigurations.

The focus of this lab is on **user privilege boundaries, credential exposure, and permission-based escalation paths**, not exploit development.

---

## 🎯 Learning Objectives
- Understand multi-stage privilege escalation paths
- Identify stepping stone users through system enumeration
- Recognize how permission differences enable escalation
- Exploit SUID binaries accessible only to specific users
- Map attacker behavior to MITRE ATT&CK and NIST CSF

---

## 🧠 Key Concepts Demonstrated
- Privilege separation between service accounts and users
- SUID enumeration and access-based visibility differences
- Credential recovery and user impersonation
- Escalation via trusted binaries
- Filesystem permission analysis

---

## 🛠️ Tools & Techniques (High-Level)
- Linux filesystem enumeration  
- SUID permission discovery  
- User and password artifact analysis  
- Credential-based user switching  
- Privileged shell execution  

---

## 🔍 Step Summary (Selected)

### 1️⃣ Initial Access as Low-Privileged Service User
```bash
whoami
```
Confirmed initial execution context as a web service account, simulating access gained through a web vulnerability.

### 2️⃣ SUID Enumeration as www-data
```bash
find / -user root -perm -4000 -print 2>/dev/null
```

Searched for root-owned SUID binaries accessible to the initial user. No directly exploitable binaries were available at this privilege level.

### 3️⃣ Identification of Potential Stepping Stone User
```bash
ls -alh /home
```
Enumerated home directories and identified an additional local user account (alice) with a dedicated home directory.

### 4️⃣ User Information Enumeration
```bash
grep alice /etc/passwd
```
Confirmed the existence of the alice user and collected account metadata relevant for further analysis.

### 5️⃣ Credential Recovery and User Escalation
```bash
su - alice
```

Recovered credentials were used to authenticate as the alice user, enabling access to files and directories previously restricted.

### 6️⃣ SUID Enumeration as Stepping Stone User
```bash
find / -user root -perm -4000 -print 2>/dev/null

```

Repeating SUID enumeration under the alice context revealed additional binaries not visible to the initial service account.

### 7️⃣ Privilege Escalation via SUID Binary
```bash
/var/bin/php -r "pcntl_exec('/bin/sh', ['-p']);"

```

Execution of a root-owned SUID binary resulted in a privileged shell while preserving effective root permissions.

8️⃣ Root Access Verification
```bash
whoami

```

Confirmed successful escalation to the root user.

### 9️⃣ Permission Boundary Validation
```bash
ls -ld /var/bin

```

Verified directory ownership and permissions, explaining why the SUID binary was only discoverable and executable after escalating to the stepping stone user.

## 🧬 MITRE ATT&CK Mapping
| Tactic               | Technique ID | Technique Name                            | Description                                       |
| -------------------- | ------------ | ----------------------------------------- | ------------------------------------------------- |
| Initial Access       | T1190        | Exploit Public-Facing Application         | Initial foothold as a web service account         |
| Discovery            | T1083        | File and Directory Discovery              | Enumeration of home directories and SUID binaries |
| Credential Access    | T1110        | Brute Force / Password Guessing           | Credential recovery enabling user impersonation   |
| Privilege Escalation | T1078        | Valid Accounts                            | Escalation to an intermediate trusted user        |
| Privilege Escalation | T1548.001    | Abuse Elevation Control Mechanism: Setuid | Exploitation of SUID binary to gain root          |
| Execution            | T1059        | Command and Scripting Interpreter         | Shell execution through trusted binaries          |
| Defense Evasion      | T1070        | Indicator Removal on Host                 | Limited logging and interactive artifacts         |

## 🧩 NIST Cybersecurity Framework (CSF) Mapping

| NIST Function | Category | Subcategory                    | Description                                                |
| ------------- | -------- | ------------------------------ | ---------------------------------------------------------- |
| Identify      | ID.AM    | Asset Management               | Identification of user accounts and privilege boundaries   |
| Identify      | ID.RA    | Risk Assessment                | Risk introduced by permission-based visibility differences |
| Protect       | PR.AC    | Access Control                 | Inadequate restriction of SUID binaries                    |
| Protect       | PR.PT    | Protective Technology          | Improper privilege separation enforcement                  |
| Detect        | DE.CM    | Security Continuous Monitoring | Lack of alerts for lateral privilege escalation            |
| Respond       | RS.AN    | Analysis                       | Analysis of escalation chain using trusted users           |
| Recover       | RC.IM    | Improvements                   | Lessons learned to harden user and binary permissions      |

## 🔐 Security Impact
This lab demonstrates how privilege escalation can occur incrementally, even when direct root escalation is not possible:
Service account compromise enabled user enumeration


Intermediate user access exposed new attack surface


SUID misconfiguration enabled final root escalation


Each step relied on legitimate system behavior, not memory corruption or exploit payloads.

## 🛡️ Defensive Recommendations
Restrict SUID binaries to only required users


Regularly audit filesystem permissions by user context


Enforce strict separation between service and user accounts


Monitor authentication events across privilege boundaries


Apply least privilege consistently across directories


Review ownership and access controls for non-standard binaries



📌 Key Takeaway
Privilege escalation is often a chain of small misconfigurations, not a single flaw.
When attackers cannot go directly to root, they will:
Look for a stepping stone user


Abuse permission differences


Escalate incrementally using trusted system behavior


Strong access control and visibility management are critical to preventing this class of attack.

