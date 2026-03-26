# Windows Command Line

# 🖥️ **Windows Command’s with Examples**

---

## 🔧 **System Info & Configuration Commands**

### ✅ `set`

- Shows all environment variables.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    set
    set PATH
    
    ```
    

---

### ✅ `ver`

- Displays the Windows OS version.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    ver
    
    ```
    
    Output: `Microsoft Windows [Version 10.0.19045.3324]`
    

---

### ✅ `systeminfo`

- Shows detailed system info: OS version, boot time, memory, etc.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    systeminfo
    
    ```
    

---

## 🌐 **Network Commands**

### ✅ `ipconfig`

- Shows IP address, subnet, and gateway.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    ipconfig
    
    ```
    

### ✅ `ipconfig /all`

- Shows **detailed network info**, including MAC address and DNS.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    ipconfig /all
    
    ```
    

---

### ✅ `tracert <host>`

- Traces route packets take to reach a host (e.g., Google).
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    tracert google.com
    
    ```
    

---

### ✅ `nslookup`

- Resolves domain names to IPs and tests DNS.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    nslookup google.com
    
    ```
    

---

### ✅ `netstat`

- Shows network connections, listening ports, routing tables.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    netstat
    
    ```
    

### ✅ `netstat -abon`

- Shows all connections with ports, process ID, and executable name.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    netstat -abon
    
    ```
    

---

## 📁 **File System & Directory Commands**

### ✅ `cd`

- Change directory.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    cd C:\Users
    
    ```
    

### ✅ `cd ..`

- Go up one directory level.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    cd ..
    
    ```
    

---

### ✅ `dir`

- Lists contents of current folder.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    dir
    dir /s  → lists subdirectories too
    dir *.txt  → only .txt files
    
    ```
    

---

### ✅ `tree`

- Shows folder structure as a tree.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    tree
    
    ```
    

---

### ✅ `mkdir` or `md`

- Make a new directory.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    mkdir MyFolder
    
    ```
    

---

### ✅ `rmdir` or `rd`

- Remove (delete) a folder (must be empty).
- 📌 Example:
    
    ```
    c
    CopyEdit
    rmdir MyFolder
    
    ```
    

---

### ✅ `del` or `erase`

- Delete a file.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    del file.txt
    del *.log
    
    ```
    

---

### ✅ `copy`

- Copy files.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    copy report.txt D:\Backups\
    
    ```
    

---

### ✅ `move`

- Move or rename files.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    move notes.txt D:\Docs\
    
    ```
    

---

### ✅ `type`

- Displays contents of a text file.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    type file.txt
    
    ```
    

---

### ✅ `more |` and `more`

- Breaks output page-by-page.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    dir | more
    
    ```
    
- **Press Spacebar** to go to the next page.

---

## ⚙️ **Task & Process Management**

### ✅ `tasklist`

- Lists running processes.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    tasklist
    
    ```
    

### ✅ `tasklist /?`

- Shows help options.

### ✅ `tasklist /FI`

- Filter tasks.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    tasklist /FI "IMAGENAME eq chrome.exe"
    
    ```
    

---

### ✅ `taskkill /PID <pid>`

- Kill a process by its PID.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    taskkill /PID 1234
    
    ```
    

---

## ⚙️ **System Maintenance**

### ✅ `chkdsk`

- Checks and fixes disk errors.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    chkdsk C: /f
    
    ```
    

---

### ✅ `driverquery`

- Lists installed drivers.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    driverquery
    
    ```
    

---

### ✅ `sfc /scannow`

- System File Checker — scans and repairs corrupt system files.
- 📌 Example:
    
    ```
    cmd
    CopyEdit
    sfc /scannow
    
    ```
    

---

## 🔌 **Shutdown & Restart**

### ✅ `shutdown` options:

| Command | Action |
| --- | --- |
| `shutdown /s` | Shutdown |
| `shutdown /r` | Restart |
| `shutdown /a` | Abort shutdown |
- 📌 Examples:
    
    ```
    cmd
    CopyEdit
    shutdown /s /t 60  → shuts down in 60 sec
    shutdown /a       → cancels shutdown
    
    ```
    

---

## ✳️ **Wildcard Use ( and `?`)**

### → any characters

### `?` → one character

### 📌 Examples:

```
cmd
CopyEdit
del *.log         → deletes all .log files
copy file?.txt D:\ → matches file1.txt, file2.txt, etc.

```

---

## 📘 Summary Table

| Command | Purpose |
| --- | --- |
| `set` | Show environment variables |
| `ver` | Show OS version |
| `systeminfo` | Full system report |
| `ipconfig` / `ipconfig /all` | Network configuration |
| `tracert` / `nslookup` | Trace and resolve network paths |
| `netstat` / `netstat -abon` | Show network ports & connections |
| `cd`, `dir`, `tree`, `mkdir`, `rmdir`, `copy`, `move`, `del` | File system operations |
| `tasklist`, `taskkill` | Process management |
| `sfc /scannow`, `chkdsk`, `driverquery` | System repair/tools |
| `shutdown /s /r /a` | Shutdown or restart control |

---