# Cyber Security

## Cyber Security Principles

The CIA Triad – 3 Pillars of Security

- Confidentiality – Keeping information private (e.g., encrypting files)
- Integrity – Ensuring data isn't tampered with (e.g., using checksums/hashes)
- Availability – Keeping systems running (e.g., backups, anti-DDoS)

AAA – Authentication, Authorization, Accounting

- Authentication – Proving your identity (username & password)
- Authorization – Determining access rights (admin vs. regular user)
- Accounting – Tracking who did what and when

---

# **Hacker’s Methodology: Step-by-Step with Explanation**

---

### 🔍 1. **Reconnaissance (Information Gathering)**

**Goal**: Collect information about the target — IPs, domains, emails, etc.

- **Passive Recon**: Without interacting with the target (e.g., WHOIS, Google dorking, social media).
- **Active Recon**: Interacting with the target (e.g., Nmap scanning, banner grabbing).

🛠️ Tools: `whois`, `nslookup`, `theHarvester`, `Maltego`, `Nmap`, `Recon-ng`

---

### 📖 2. **Scanning & Enumeration**

**Goal**: Identify open ports, services, versions, users, and vulnerabilities.

- **Port Scanning**: Identify services (e.g., HTTP on port 80)
- **Service Enumeration**: Find versions and misconfigurations
- **OS Fingerprinting**: Identify operating system

🛠️ Tools: `Nmap`, `Nikto`, `Netcat`, `Enum4linux`, `Dirb`, `Gobuster`

---

### 💥 3. **Gaining Access (Exploitation)**

**Goal**: Exploit vulnerabilities to get into the system.

- **Common Attack Types**:
    - SQL Injection
    - Remote Code Execution (RCE)
    - Buffer Overflows
    - Credential Exploits (e.g., default passwords)

🛠️ Tools: `Metasploit`, `SQLmap`, `Hydra`, `ExploitDB`

---

### 🐚 4. **Maintaining Access (Persistence)**

**Goal**: Stay inside the system even after reboot or detection.

- Create backdoors or scheduled tasks
- Install rootkits or persistent shells

🛠️ Tools: `Netcat`, `Meterpreter`, `Empire`, `Crontab`, `SSH keys`

---

### 🔐 5. **Privilege Escalation**

**Goal**: Elevate from a basic user to admin/root.

- Exploit misconfigurations, weak permissions, outdated kernels
- Look for stored credentials or password reuse

🛠️ Tools: `LinPEAS`, `WinPEAS`, `GTFOBins`, `sudo -l`, `kernel exploits`

---

### 📦 6. **Covering Tracks**

**Goal**: Remove evidence of the attack.

- Delete logs
- Clear shell history
- Mask malware as legit processes

🛠️ Tools: `shred`, `log cleaner scripts`, `history -c`, `rootkits`

---

### 📤 7. **Data Exfiltration**

**Goal**: Steal or extract sensitive data.

- Database dumps
- Credential theft
- File downloads

🛠️ Tools: `scp`, `FTP`, `HTTP POST`, `DNS tunneling tools`

---

### 👨‍⚖️ 8. **Reporting (for ethical hackers)**

**Goal**: Document vulnerabilities, steps taken, and how to fix them.

- Include risk levels, proof of concepts (PoCs), and remediation steps.

🛠️ Tools: `Dradis`, `Serpico`, custom documentation

---

### 🗺️ Bonus: The Hacker’s Mindset

- Curiosity > Malice
- Think like an attacker to protect like a defender
- Always look for unintended behavior, misconfigurations, and human error

# The OWASP Top 10 (2021 Edition)

| Vulnerability | Description |
| --- | --- |
| Broken Access Control | Users can act outside their permissions. |
| Cryptographic Failures | Poor protection of sensitive data (e.g., weak encryption). |
| Injection | Attacker sends malicious data to execute unintended commands (e.g., SQL injection). |
| Insecure Design | Flaws in the design phase causing security issues. |
| Security Misconfiguration | Incorrectly configured security settings. |
| Vulnerable and Outdated Components | Using components with known vulnerabilities. |
| Identification and Authentication Failures | Weaknesses in login or session management. |
| Software and Data Integrity Failures | Code and data that can be maliciously altered. |
| Security Logging and Monitoring Failures | Lack of logs or alerts to detect attacks. |
| Server-Side Request Forgery (SSRF) | Server is tricked into making unauthorized requests. |

## TOPICS

[Networking ](Cyber%20Security/Networking%2028308abb572d805daad7c30767519558.md)

[Linux](Cyber%20Security/Linux%2028308abb572d8059af73f8e0daf98eb1.md)

[Cyber Security Tools ](Cyber%20Security/Cyber%20Security%20Tools%2028308abb572d80248911f58283090730.md)

[Windows](Cyber%20Security/Windows%2028308abb572d80f8867affc39de9a252.md)

[Web Application](Cyber%20Security/Web%20Application%2026108abb572d80c794e6c4eaac1bdf13.md)

[Over The Wire ](Cyber%20Security/Over%20The%20Wire%2023408abb572d802aaf7ef37e849e9a46.md)

---