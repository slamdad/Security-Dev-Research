# Powershell

[PowerShell Commands & Scripts.pdf](Powershell/PowerShell_Commands__Scripts.pdf)

[Untitled spreadsheet.xlsx](Powershell/Untitled_spreadsheet.xlsx)

That's a very comprehensive list of commands and topics, primarily spanning **PowerShell scripting fundamentals** and **Network/Active Directory Enumeration** (common in penetration testing/red teaming).

I will organize the core PowerShell commands based on their purpose and explain how they are typically combined in a post-exploitation or reconnaissance workflow. I'll exclude the direct exploitation commands like those downloading Meterpreter.

---

## 💻 PowerShell & Network Reconnaissance Command Map

### 1. PowerShell Fundamentals & Data Handling

These commands are used to manage data, files, and the PowerShell environment itself.

| Command(s) | Purpose | Combination Example |
| --- | --- | --- |
| **`Get-Command`** | Lists all available cmdlets, functions, and aliases. | `(Get-Command |
| **`Get-Location`, `Set-Location`** | Views and changes the current directory path. | `if(Set-Location C:\...){Write-Host "It exists!"}` (Checks if a path exists and changes directory.) |
| **`out-file`, `out-gridview`** | Directs command output to a file or an interactive GUI table. | `Get-NetUser |
| **`foreach`, `Start-Process`** | `foreach`: Iterates over a collection. `Start-Process`: Executes an external program or script. | `foreach($i in $array) { Start-Process notepad.exe -ArgumentList $i }` (Opens Notepad for each item.) |
| **`[System.Text.Encoding]::...`** | .NET calls used for data manipulation, often for **Typecasting** or decoding. | `[System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String("$MyBase64"))` (Decodes Base64 data.) |
| **`Invoke-WebRequest`**, **`(New-Object System.Net.WebClient).DownloadFile`** | Retrieves content or downloads a file from a URL. | `Invoke-WebRequest -Uri "http://..." -OutFile "file.txt"` (Downloads a file.) |

Export to Sheets

---

### 2. Local File & System Enumeration

These commands are used to find specific files, read content, check permissions, and analyze running processes.

| Command(s) | Purpose | Combination Example |
| --- | --- | --- |
| **`Get-ChildItem` (alias `dir`, `ls`)** | Finds files/folders, often used with `-Recurse` to search subdirectories. | `Get-ChildItem “*interesting-file*” -Path C:\ -Recurse -ErrorAction SilentlyContinue` (Searches the whole C: drive for a specific file pattern.) |
| **`Get-Content`, `Select-String`** | `Get-Content`: Reads the text inside a file. `Select-String`: Searches file content for a text pattern. | `Get-ChildItem ... |
| **`Get-FileHash`** | Calculates the cryptographic hash of a file. | `Get-ChildItem ... |
| **`Get-Process`** | Lists currently running processes. | `(Get-Process).Count` (Counts how many processes are running.) |
| **`Get-Acl`** | Retrieves security information (like ownership and permissions) for a file or folder. | `(Get-Acl -Path “C:\”).Owner` (Finds the owner of the root C: drive.) |
| **`Get-ScheduledTask`** | Lists and queries scheduled tasks. | `(Get-ScheduledTask |

Export to Sheets

---

### 3. Local User & Group Enumeration

These commands help map local accounts, groups, and security settings on the host.

| Command(s) | Purpose | Combination Example |
| --- | --- | --- |
| **`Get-LocalUser`** | Lists and queries local user accounts. | `(Get-LocalUser).Name.Count` (Counts local users.) |
| **`Get-LocalGroup`** | Lists and queries local security groups. | `(Get-LocalUser |

Export to Sheets

---

### 4. Network and Connection Enumeration

These commands are crucial for mapping the network, finding open ports, and identifying the system's patching status.

| Command(s) | Purpose | Combination Example |
| --- | --- | --- |
| **`Get-NetIPAddress`** | Shows the system's IP address and interface details. | `Get-NetIPAddress` |
| **`Get-NetTCPConnection`** | Displays active TCP connections and listening ports. | `(Get-NetTCPConnection |
| **`ping` / `Select-String ttl`** | Standard ping utility combined with PowerShell filtering to identify active hosts. | `1..15 |
| **`New-Object Net.Sockets.TcpClient`** | .NET call used to perform a TCP port scan. | `1..1024 |
| **`Get-HotFix`** | Lists installed security updates and patches (KBs). | `(Get-HotFix).Count` (Counts total installed patches.) |

Export to Sheets

---

### 5. Active Directory (AD) Enumeration (PowerView)

These commands are used to map the domain environment, identify high-value targets, and look for privilege escalation paths.

| Command(s) | Purpose | Combination Example |
| --- | --- | --- |
| **`Get-NetUser`** | Enumerates domain user accounts. | `get-netuser |
| **`Get-NetGroupMember`** | Lists members of a specific domain group. | `Get-NetGroupMember -GroupName "Domain Admins"` (Identifies high-privilege users.) |
| **`Get-NetDomainController`** | Finds the IP and hostname of the Domain Controller. | `Get-NetDomainController` |
| **`Find-DomainShare`** | Finds network shares and checks the current user's access rights. | `Find-DomainShare” -CheckShareAccess` |
| **`Get-NetGPO`** | Enumerates Group Policy Objects. | `Get-NetGPO` (Used to find policy-based weaknesses or user-created policies.) |
| **`Find-LocalAdminAccess`** | Checks which machines the current user has local administrator rights on. | `Find-LocalAdminAccess` |

Export to Sheets

---

## 🔄 The Typical Red Team Flow

These commands are often executed in a logical progression during a targeted assessment:

1. **Initial Foothold & System Check:**
    - **Payload Delivery:** Use `Invoke-WebRequest` or `(New-Object System.Net.WebClient).DownloadFile` to download tools/scripts.
    - **System Status:** Use `Get-Process` and `Get-HotFix` to assess process integrity and patch level for privilege escalation ideas.
2. **Local Reconnaissance:**
    - **Local Users:** Use `Get-LocalUser` and `Get-LocalGroup` to understand local accounts.
    - **File Hunting:** Use `Get-ChildItem ... | Select-String "password"` to look for credentials or secrets in files.
    - **Network Status:** Use `Get-NetTCPConnection` to find listening services and establish network boundaries.
3. **Network Mapping:**
    - **Ping Sweep:** Use the `1..15 | %{...ping...}` command to find active hosts in the local subnet.
    - **Port Scan:** Use the `1..1024 | %{...TcpClient...}` command to identify open services on high-interest targets.
4. **Domain Enumeration (If AD is present):**
    - **Key Targets:** Use `Get-NetDomainController` to find the DC.
    - **Privilege Audit:** Use `Get-NetGroupMember -GroupName "Domain Admins"` to identify targets for credential harvesting or abuse.
    - **GPO/Trusts:** Use `Get-NetGPO` and `Get-NetDomainTrust` to find misconfigurations or access paths to other domains.