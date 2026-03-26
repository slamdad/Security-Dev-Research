# Windows Active Directory

# 🧠 **Windows Domain & Active Directory with Step-by-Step Examples**

---

## 1. 💼 **What is a Windows Domain?**

A centralized network where user accounts, devices, and policies are managed by a **Domain Controller (DC)**.

> 🔹 Example: Instead of managing each PC separately, an IT admin can manage all 500 office PCs and users from a single server.
> 

---

## 2. 🧱 **What is Active Directory (AD)?**

A Microsoft service that stores info about users, computers, and resources, enabling central management.

### 🧰 Admin Tool:

- **Active Directory Users and Computers (ADUC)**

> 🧭 Access via:
> 
> 
> `Start > Run > dsa.msc`
> 

---

## 3. 👤🖥️ **Creating Users and Computers**

### 👤 Create a New User (ADUC):

1. Open **Active Directory Users and Computers (dsa.msc)**.
2. Right-click an **OU** (e.g., `Sales`) → `New` → `User`.
3. Enter:
    - First name: `John`
    - Username: `john.smith`
4. Set an initial password (optionally require password change at next login).
5. Click **Finish**.

✅ Done! User can now log in from a domain PC.

---

### 🖥️ Join a Computer to Domain (From Client PC):

1. Right-click **This PC** → `Properties`.
2. Click **"Rename this PC (advanced)"**.
3. Click **"Change..."** > Select **Domain**, enter:
    
    ```
    company.local
    
    ```
    
4. Enter domain admin credentials.
5. Restart the PC.

✅ Now the computer appears in AD under **"Computers"** or the assigned OU.

---

## 4. 👥 vs 🗂️ **Groups vs Organizational Units (OUs)**

### 🗂️ Create an Organizational Unit (OU):

1. In ADUC: Right-click the domain → `New` → `Organizational Unit`.
2. Name it: `HR`
3. You can now move users/computers into this OU and apply **Group Policies**.

---

### 👥 Create a Group:

1. Right-click an OU (e.g., `HR`) → `New` → `Group`
2. Name: `HR_SharedDrive_ReadOnly`
3. Scope: Global
4. Type: Security
5. Click OK

> ✅ Use this group to assign folder permissions or email access.
> 

---

## 5. 👨‍💼 **Managing Users**

### ✅ Reset Password:

- Right-click user → `Reset Password`
- Enter new password

### ✅ Force Logoff or Disable Account:

- Right-click user → `Disable Account`
- Or: Use `Logoff` if using Remote Desktop Services

---

### 💻 PowerShell Example: Create a New User

```powershell
New-ADUser `
  -Name "John Smith" `
  -SamAccountName "john.smith" `
  -UserPrincipalName "john.smith@company.local" `
  -AccountPassword (ConvertTo-SecureString "Pa$$w0rd123" -AsPlainText -Force) `
  -Enabled $true `
  -Path "OU=HR,DC=company,DC=local"

```

---

## 6. 🖥️ **Managing Computers**

### ✅ Move to Specific OU:

1. In ADUC: Right-click a computer → `Move`
2. Choose the appropriate OU (`e.g., HR\Workstations`)

### ✅ Rename Computer (via PowerShell):

```powershell
Rename-Computer -NewName "HR-PC-01" -Restart

```

---

## 7. ⚙️ **Applying Group Policies (GPOs)**

### Open Group Policy Management:

- Run `gpmc.msc`

### Create and Link a GPO:

1. Right-click the OU (e.g., `HR`) → `Create a GPO in this domain, and Link it here...`
2. Name it: `HR Desktop Policy`

### Edit the GPO:

- Right-click GPO → `Edit`
- Example:
    
    `User Configuration > Policies > Administrative Templates > Desktop > Desktop Wallpaper`
    
- Set wallpaper path, click OK.

> ✅ Now all users in the HR OU will get this wallpaper.
> 

---

## 8. 🔐 **Authentication Methods**

### AD Authentication uses:

- **Kerberos** (default, fast & secure)
- **NTLM** (fallback, older)
- Works for:
    - Logging into PCs
    - Accessing file shares
    - Running services

---

## 9. 🌳🌲🤝 **Domains, Forests, and Trusts**

### 🧱 Domains:

- A single AD instance (e.g., `sales.company.com`)

### 🌲 Forests:

- Multiple domains under a single logical umbrella.
- Share schema, trust, global catalog.

### 🤝 Trusts:

- Set up in **Active Directory Domains and Trusts (domain.msc)**
- Right-click domain → `Properties` → `Trusts` tab

---

## ✅ Quick Summary Table with Commands

| Task | Tool | GUI Path / Command |
| --- | --- | --- |
| Create User | ADUC | `dsa.msc` → OU → Right-click → New → User |
| Create OU | ADUC | `Right-click Domain > New > Organizational Unit` |
| Join PC to Domain | System Settings | `System > Rename this PC > Join domain` |
| Add User via PowerShell | PowerShell | `New-ADUser ...` |
| Create Group | ADUC | `Right-click OU > New > Group` |
| Move Computer to OU | ADUC | `Right-click Computer > Move` |
| Create GPO | GPMC | `gpmc.msc > OU > Right-click > Create GPO and Link` |
| Edit GPO | GPMC | `Right-click GPO > Edit` |

---