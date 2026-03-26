# Windows Log Management

# Windows: main Event IDs, malicious uses, and user management structure

I’ll summarize high-value events and why defenders care.

## Core Windows Security Event IDs (common ones)

- **4624** — Successful account logon. (Use: detect unusual logon times, new sources, lateral movement via RDP/SMB)
- **4625** — Failed logon. (Use: brute force detection)
- **4634** — Logoff.
- **4672** — Special privileges assigned to new logon (e.g., admin). (Use: indicates privilege escalation)
- **4688** — New process created (very important). (Use: detect suspicious processes launching)
- **4689** — Process ended.
- **4656 / 4657** — Object access handle created and object modified (file handle / file change). (Use: detect files dropped/modified by malware)
- **4663** — An attempt was made to access an object (file). (High value if monitoring sensitive paths)
- **5140** — Network share object accessed.
- **5156** — Windows Filtering Platform permit (network connection allowed) — kernel-level network events.
- **1102** — Audit log cleared. (Indicator of log tampering)
- **4720** — User account created.
- **4722** — User account enabled.
- **4723/4724** — Password change/reset attempts.

(Events vary by Windows version and audit policy; many auditing categories are disabled by default — enable what you need.)

**Malicious usage reasons**

- File/registry modifications (4656/4657/4663) — malware drops files or modify registry for persistence.
- Process creation (4688) — detect living-off-the-land binaries, suspicious command lines (powershell with encoded commands, e.g. `EncodedCommand`).
- Network events (5156 / Sysmon NetworkConnect) — detect C2 or data exfil.
- Account creation/privilege events (4720/4672) — adversary creating accounts or elevating privileges.

## Structure of user management (brief)

- **Accounts**: local accounts, domain accounts (Active Directory). Each has an SID (security identifier).
- **Groups**: Administrators, Domain Admins, Power Users. Group membership drives access.
- **UAC**: User Account Control separates elevated admin token — actions requiring admin prompt.
- **Service accounts**: often used for scheduled tasks or services; compromise grants persistence.
- **Service Principal Names (SPNs)** and Kerberos tickets — target for lateral movement (golden ticket, pass-the-ticket tactics).

# Sysmon — explain processes/events/files/network

Sysmon is a powerful Windows agent (Sysinternals) that logs high-fidelity events not always in Windows Security log. Install via a config (XML) which controls what to collect.

**Key Sysmon event types**

- **Event ID 1** — Process Create (command line and parent process). Extremely high value.
- **Event ID 2** — File creation time changed.
- **Event ID 3** — Network connection (process-level: source exe, dest IP/port).
- **Event ID 6** — Driver loaded.
- **Event ID 7** — Image (executable) loaded — useful for detecting DLL loads.
- **Event ID 8** — CreateRemoteThread — suspicious for code injection.
- **Event ID 11** — File created. (Often used to detect dropped payloads.)
- **Event ID 12** — Registry value set.
- **Event ID 13** — Registry value deleted.
- **Event ID 22** — DNS query (process-related) — good for spotting unusual DNS lookups.
- **Event ID 4624/4688 mapping** — Sysmon’s events complement Security log by adding process-level context and command lines.

**Why Sysmon?**

- Gives *process→network→file* linking (which process opened a connection, which file it dropped), which Security logs often don’t include.
- Detects process injection, suspicious child-parent trees (e.g., `svchost` spawning `powershell` with encoded args).

**Practical notes**

- Start with a curated sysmon config (Yara-like rules) — whitelist known good software, monitor unknowns.
- Store `CommandLine`, `ParentImage`, `ImageLoaded`, and `Hashes` — they help in triage.

# Your table mapping (interpreting)

You provided a mapping like:

- (File Create / Registry Value Set) → security alternatives 4656/4657
- (Network Connection / DNS Query) → no direct Security log alternative (needs firewall/DNS)

Interpretation:

- Many Sysmon detections (file create, registry set, network connection) have *no single* equivalent in Windows Security logs, or the Security logs are less descriptive. That’s why Sysmon + Windows Event Log + network/DNS telemetry + EDR = richer picture.

# PSReadline history file

`C:\Users\<USER>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`

- Contains the command history typed in interactive PowerShell sessions (plain text).
- **Privacy/security risk**: can contain credentials typed directly, long command-line strings (including encoded commands). On compromised host, an attacker can inspect this for secrets.
- Mitigation: clear history, configure PSReadline to not persist, or securely rotate machines/users