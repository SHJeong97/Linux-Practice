# 🚨 Open Port Discovery Challenge (Rogue Service Flag)

## 📌 Overview
In this challenge, I acted as a junior System Administrator responding to an alert for unusual network activity on an internal development server. The objective was to identify unauthorized services by scanning localhost for open TCP ports, then interact with the suspicious service to retrieve a confirmation flag from a hidden endpoint.

---

## 🎯 Objectives
- Scan localhost for open TCP ports using Nmap
- Identify an unusual/non-standard open port
- Interact with the discovered service using curl
- Retrieve the flag from the `/flag` endpoint

---

## 🧠 Skills Demonstrated
- Host-based network triage
- Port and service discovery with Nmap
- HTTP service enumeration with curl
- Incident validation via artifact retrieval
- Basic investigation workflow for rogue services

---

## 🛠️ Tools Used
- `nmap` (port scanning / service discovery)
- `curl` (HTTP interaction / endpoint testing)
- Linux shell

---

## 🧪 Methodology

1. Perform a TCP port scan on localhost to identify open services.
2. Review results and locate the unusual/non-standard open port.
3. Use curl to query the service root endpoint.
4. Request the `/flag` endpoint to retrieve the breach confirmation flag.

---

## ✅ Used Command

```bash
# 1) Scan localhost for open TCP ports
nmap localhost

# (Optional) Faster scan with default top ports + service detection
# nmap -sV localhost

# 2) Interact with the discovered service on the suspicious port
# Replace <PORT> with the unusual open port found in your scan
curl http://localhost:<PORT>

# 3) Retrieve the flag from the required endpoint
curl http://localhost:<PORT>/flag

```
## 🏁 Result
A non-standard open TCP port was identified on localhost, indicating an unauthorized or rogue service. Interacting with the service and accessing the /flag endpoint successfully returned a flag in the required format:
FLAG{...}

