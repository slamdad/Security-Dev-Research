# Port Swigger

## 📌 Overview

### 🔹 What is PortSwigger?

PortSwigger is a cybersecurity company best known for creating **Burp Suite**, a popular tool for **web application security testing**. They also run the **PortSwigger Web Security Academy**, a free online training platform.

---

## 🎓 PortSwigger Web Security Academy

### 🔹 What is it?

The **Web Security Academy** is a **free educational resource** by PortSwigger to help you learn web application security through **theory and hands-on labs**.

### 🔹 Features:

- **Interactive labs** (hosted environments to test vulnerabilities)
- **Beginner to Advanced** content
- Covers OWASP Top 10 vulnerabilities and more

### 🔹 Topics Covered:

- SQL Injection
- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- Authentication vulnerabilities
- Business logic flaws
- Web caching, SSRF, and more

# TOPICS

# CHECK THE LABS IN PORTSWIGGER

# Authentication

### What It Is:

Authentication verifies a user’s identity, usually via usernames and passwords (or other factors like 2FA). Weaknesses here include username enumeration, brute-forcing, or bypassing 2FA.

### Apprentice-Level Topics & Steps:

- **Username enumeration**: Detect valid usernames via differing error messages.
- **Brute-force protection flaws**: See how weak rate-limiting or IP blocking can fail.
- **2FA simple bypass**: Basic weaknesses in two-factor logic.
- **Password reset broken logic**: Flaws when resetting passwords, e.g., insecure tokens.

# Information Disclosure

### What It Is:

These are vulnerabilities that expose confidential data—framework versions, paths, debug info, or source files—through error messages, debug pages, or backup files.

### Apprentice-Level Labs:

- Error messages revealing sensitive framework info.
- Debug pages exposing back-end details.
- Backup files revealing source code.
- Authentication bypass via header disclosure.

# Path Traversal

### What It Is:

This vulnerability lets attackers read arbitrary files (e.g., `/etc/passwd`) by manipulating file path parameters (e.g., `?file=../../etc/passwd`).

### Apprentice-Level Labs:

- Reading arbitrary files in a simple setup.

# **Broken Access Control (Access Control Vulnerabilities)**

### What It Is:

This occurs when users can bypass restrictions—such as accessing admin pages, other users' data, or skipping workflow steps.

### Key Concepts:

- **Vertical escalations**: Normal user accessing admin functions.
- **Horizontal escalations**: Accessing other users' data (IDORs).
- **Context-based issues**: Skipping steps or relying on referer headers.

### Apprentice-Level Labs:

- Unprotected admin functionality (simple).
- User role controlled by request parameter.
- Accessing admin via unpredictable URL.

# Business Logic Vulnerabilities

### What It Is:

These aren't technical bugs but logical flaws in application workflows—like trust in client-side controls, unintended negative quantities, or coupon misuse.

### Apprentice-Level Labs:

- **Excessive trust in client-side controls**.
- **High-level logic flaws** (e.g., negative quantities).
- **Inconsistent security controls**.
- **Flawed business rule enforcement** (e.g., reusing coupons).

# SQL Injection

## What is SQL Injection?

- **SQL Injection (SQLi)** is a web security vulnerability that allows an attacker to **interfere with queries** an application makes to its database.
- It happens when **unsanitized user input** is concatenated into SQL queries.
- Attackers can:
    - Bypass login mechanisms
    - Read or modify database data
    - Extract sensitive information (users, passwords, emails)
    - Sometimes gain full control of the server

---

## 🔹 How does it work?

Suppose a login form executes this query:

```sql
SELECT * FROM users WHERE username = 'input' AND password = 'input';

```

If the user enters:

```
' OR '1'='1

```

The query becomes:

```sql
SELECT * FROM users WHERE username = '' OR '1'='1' AND password = '';

```

Since `'1'='1'` is always true, the attacker bypasses authentication.

---

## 🔹 Types of SQL Injection

1. **In-band SQLi** (classic, easiest):
    - Attacker sees results directly in the response
    - Uses `UNION`, `ORDER BY`, etc.
2. **Blind SQLi**:
    - Data isn’t shown directly, but attacker infers by responses (true/false, timing)
    - Example: `AND 1=1` vs `AND 1=2`
3. **Out-of-band SQLi**:
    - Attacker triggers database to send data externally (e.g., via DNS request)

---

## 🔹 ORDER BY in SQLi

- Used to **determine the number of columns** in a query.
- Example:

```sql
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
' ORDER BY 4--

```

➡️ When you go beyond the actual column count, the server throws an error.

This helps attackers find out how many columns the query returns.

---

## 🔹 UNION in SQLi

- `UNION` lets attackers combine results of two queries.
- Example:

```sql
' UNION SELECT NULL,NULL--

```

- Steps with UNION:
    1. **Find number of columns** (using `ORDER BY` or trial with `UNION SELECT NULL`).
    2. **Check which column accepts text**:
        - Example:
            
            `' UNION SELECT 'abc',NULL,NULL--`
            
    3. **Extract database info**:
        - Current database user:
            
            ```sql
            ' UNION SELECT NULL,user(),NULL--
            
            ```
            
        - Database name:
            
            ```sql
            ' UNION SELECT NULL,database(),NULL--
            
            ```
            
        - Table names (MySQL):
            
            ```sql
            ' UNION SELECT NULL,table_name,NULL FROM information_schema.tables--
            
            ```
            
        - Column names:
            
            ```sql
            ' UNION SELECT NULL,column_name,NULL FROM information_schema.columns WHERE table_name='users'--
            
            ```
            
        - Extract data:
            
            ```sql
            ' UNION SELECT NULL,username||':'||password,NULL FROM users--
            
            ```
            

---

## 🔹 Key Notes

- Always end injections with `-` (comment) to ignore trailing SQL.
- Use `NULL` when testing with UNION to match data types.
- Modern apps often block basic SQLi — but misconfigurations still exist.
- SQLi can escalate to **RCE (Remote Code Execution)** in severe cases.

---

✅ **In summary**:

- SQL Injection manipulates SQL queries using unsanitized input.
- `ORDER BY` helps find column count.
- `UNION SELECT` helps extract data by merging attacker-controlled queries.

# OS command injection

- **What:** Attacker injects OS-level commands into app input that get executed by the server (e.g., `; ls`, `&& cat /etc/passwd`, `| whoami`).
- **Impact:** Remote command execution, data theft, privilege escalation, pivoting.
- **Typical sinks:** `system()`, `exec()`, `popen()`, backticks ``...``, `shell_exec()`, CLI wrappers (image processing, ping/traceroute).
- **Common payloads:**
    - Simple: `; whoami` / `&& id`
    - Pipe: `| ls -la /`
    - Backticks: ``cat /etc/passwd``
    - Blind (timing): `; sleep 5` or `&& ping -c 5 attacker.com`
- **Testing tips:**
    - Start with harmless probes: `; echo INJECT` or `&& echo INJECT` to see reflection.
    - Try separators: `;`, `&&`, `||`, `|`, `\n`, `$(...)`, backticks.
    - Use time-based payloads where output not returned.
    - Test different encodings (URL encode, double encode) and different parameter contexts (headers, cookies, filenames).
- **Detection:** Look for unexpected command output, delayed responses (timing attacks), new files/processes, network callbacks. Use Burp Repeater + Collaborator for OOB.
- **Fixes / mitigations:**
    - Avoid invoking shell; use safe APIs that take argument arrays (no shell parsing).
    - Strong input validation: allow-list known-good input.
    - Escape/sanitize when shell invocation unavoidable (but prefer removal).
    - Run services with least privilege; use chroot/containers.
    - Use application-level rate-limits/monitoring and WAF signatures.
- **Quick note:** Even seemingly innocuous inputs (filenames, email addresses) can be exploited if passed to a shell.

---

# File upload vulnerability

- **What:** Attacker uploads a crafted file to execute code, overwrite files, or exfiltrate data.
- **Impact:** Remote code execution (if file served/executed), stored XSS, malware hosting, data loss.
- **Common issues:**
    - Client-side checks only (bypassable).
    - Trusting filename/extension or MIME type.
    - Storing uploads in webroot with executable permissions.
    - Insufficient validation of file contents (magic bytes).
    - Using original filename without sanitization -> path traversal.
- **Attack techniques:**
    - Change extension to `.jpg.php` or `shell.php.jpg` (double extension).
    - Modify `Content-Type` header (MIME sniffing).
    - Upload polyglot files (image + PHP).
    - Null byte tricks (historical): `shell.php%00.jpg` (older PHP).
    - Bypass filter by embedding PHP in EXIF/comments for some processors.
    - Overwrite existing files via predictable filenames.
    - Upload large files to exhaust disk (DoS).
- **Testing tips:**
    - Try uploading `.php`, `.phtml`, `.jsp` and then access the URL.
    - Upload a benign web shell payload (e.g., `<?php phpinfo(); ?>`) to test execution (only in lab).
    - Test MIME vs magic-bytes: change header but keep file header.
    - Check upload path (response Location, returned filename).
    - Use Burp Intruder to fuzz filenames and content-types.
- **Fixes / mitigations:**
    - Validate on server-side: allow-list extensions, check magic bytes, verify image libraries can open the file.
    - Store uploads outside webroot; serve through a proxy that forces non-executable headers.
    - Rename files with safe/unpredictable names (UUIDs) and set restrictive permissions.
    - Strip executable code from images (reprocess images server-side).
    - Enforce content-disposition and `X-Content-Type-Options: nosniff`.
    - Scan uploads with antivirus and set size limits.
    - Use CSP and proper HTTP headers when serving user files.

---

# XSS (Cross-Site Scripting)

- **What:** Injection of malicious JavaScript into pages viewed by other users.
- **Types:** Reflected (non-persistent), Stored (persistent), DOM-based (client-side).
- **Impact:** Session theft, account takeover, CSRF bypass, keylogging, actions as victim.
- **Common sinks & contexts:** HTML body, attribute (`href`, `src`, `alt`), inline JS, event handlers (`onclick`), JS contexts (`eval()`, `innerHTML`), URL fragments (DOM).
- **Test payloads (start simple):**
    - `<script>alert(1)</script>`
    - `"><script>alert(1)</script>` (attribute break)
    - `'><img src=x onerror=alert(1)>`
    - `"><svg/onload=alert(1)>` (SVG)
    - DOM: `javascript:alert(1)` or injection into `location.hash` / `innerHTML`.
- **Advanced:** use polyglots, bypass filters with encoded UTF-8/HTML entities, use event handlers or `setTimeout` for CSP bypass.
- **Testing tools:** Burp Repeater, DOM Invader, browser devtools, CSP eval, Burp Collaborator for OOB.
- **Fixes / mitigations:**
    - Contextual output encoding/escaping (HTML-encode, attribute-encode, JS-encode, URL-encode depending on context).
    - Use secure frameworks that auto-escape templates.
    - Validate & sanitize input (allow-list).
    - Use `HttpOnly` cookies for session tokens, set `SameSite` cookies.
    - Implement CSP (Content-Security-Policy) to reduce JavaScript execution surface (but don’t rely on CSP as sole protection).
    - Avoid insertion of untrusted data into `innerHTML`, `eval`, `document.write`.
- **Detection:** Search for reflected payloads, stored occurrences in DB, check DOM sinks. Use automated scanners but verify manually.

---

# Command injection (general)

- **What:** Generic term for injecting input that alters program logic/commands — includes OS command injection and other command interpreters (e.g., LDAP, underlying app commands).
- **Overlap:** Subset includes OS command injection; also includes injection into system utilities, application interpreters, or shell wrappers.
- **Testing methodology:**
    - Identify where user input reaches command interpreters or system calls.
    - Try separators and control characters for the interpreter in use.
    - Use harmless probes (`; echo INJ`) and timing probes for blind cases.
    - Consider context: Windows vs Unix separators (`&`, `|`, `;`, `\n`, `&&`, `||`, `^` on Windows).
- **Payload examples (contextual):**
    - Unix: `; uname -a` / `&& id` / `$(whoami)` / ``whoami``
    - Windows: `& whoami`, `| ipconfig`, `&& dir`
- **Mitigations:** (same as OS injection) avoid shell usage, input allow-listing, argument-splitting APIs, least privilege, containerization.
- **Detection:** Monitor for command output exposure, unexpected delays, or network callbacks; use logs and IDS/WAF.

# CSRF (Cross‑Site Request Forgery)

- **What:** Attacker tricks an authenticated victim’s browser into sending unintended requests to a target site (e.g., transfer money, change email) using the victim’s credentials/cookies.
- **Impact:** Unauthorized state-changing actions (transactions, password/email changes, settings). Usually requires victim to be authenticated.

### How it works (simple flow)

1. Victim logs into `bank.com` (has auth cookie).
2. Victim visits `attacker.com`.
3. `attacker.com` causes the browser to submit a request to `bank.com` (e.g., auto-submitted form or image GET).
4. Browser includes `bank.com` cookies → action executes with victim’s privileges.

### Common CSRF vectors

- Auto-submitted HTML form (`<form action="https://bank.com/transfer" method="POST">...<input type="submit"></form><script>form.submit()</script>`).
- Image, script, or iframe GET requests for endpoints that change state.
- AJAX/fetch requests (subject to CORS restrictions; preflight often blocks POST unless CORS allowed).
- Pre-authenticated API endpoints that rely only on cookies.

### Example malicious HTML (POST form)

```html
<form action="https://bank.com/transfer" method="POST" id="f">
  <input name="to" value="attacker" />
  <input name="amount" value="1000" />
</form>
<script>document.getElementById('f').submit();</script>

```

### Testing tips

- Identify state-changing endpoints (POST/PUT/DELETE).
- Try CSRF PoC: craft auto-submitting form and see if action occurs while logged in as victim.
- Use Burp to replay legitimate requests and check if any anti-CSRF tokens exist and are required.
- Test token predictability: replay same token twice; try reusing token across sessions.

### Defenses / mitigations (strongest → weakest)

1. **Per-request CSRF tokens (Synchronizer Token Pattern)**
    - Unique, unpredictable token per user/session/forms; validate server-side on each state-changing request.
2. **SameSite cookies**
    - `SameSite=Strict` or `Lax` (Lax prevents most cross-site POSTs; Strict is stricter). `SameSite=None; Secure` needed for cross-site usage but allows CSRF.
3. **Double-submit cookie**
    - Send CSRF token in cookie and as request body/header; verify both match. Must be unguessable.
4. **Origin / Referer header checks**
    - Validate `Origin` (preferred) or `Referer` header matches expected origin; reliable for modern browsers and CORS-safe methods.
5. **Require re-authentication / 2FA for sensitive actions**
6. **Use custom request headers + CORS**
    - Browsers block cross-origin custom headers without preflight; APIs that require a custom header are safer, but CORS must still be configured securely.

### Implementation notes

- Token must be tied to session and not predictable.
- Do server-side validation; client-side checks alone are useless.
- Protect all state-changing endpoints (not just forms visible to user).
- Use HTTPS and `HttpOnly`/`Secure` on cookies where applicable.
- Avoid relying solely on obscurity (e.g., guessable tokens or unique URLs).

### Detection / indicators

- State changes triggered by third-party pages.
- Missing CSRF token or token reused/replayed.
- Referer/origin that matches attacker site during sensitive request.

---

# Clickjacking (UI redressing)

- **What:** Attacker embeds a target site (or parts of it) in a hidden/transparent iframe on a malicious page to trick users into interacting with UI elements they can’t see (e.g., clicking “Approve” while thinking they click something else).
- **Impact:** Unintended clicks, approve actions, permission grants, social engineering (e.g., enabling camera/mic), transaction approvals.

### Typical attack patterns

- Invisible iframe overlaid on attacker’s UI; victim clicks a visible button, actually clicking the target page’s button.
- Framed login forms to capture user input (UI deception) or trick to change settings.
- Drag-and-drop or cursorjacking variants to bypass visual cues.

### Simple attack demo (conceptual)

```html
<style>
  iframe { opacity:0; position:absolute; top:0; left:0; width:400px; height:200px; }
</style>
<button>Click to win prize</button>
<iframe src="https://target.com/confirm" />

```

### Testing tips

- Try embedding sensitive pages in an iframe on attacker-controlled domain; check if page blocks framing.
- Test different embedding techniques (iframe, object, embed).
- Inspect headers (`X-Frame-Options`, `Content-Security-Policy: frame-ancestors`).
- Try UI overlay attacks manually in a controlled lab.

### Defenses / mitigations

1. **HTTP header: `X-Frame-Options`**
    - `DENY` — no framing allowed.
    - `SAMEORIGIN` — allow same-origin framing only.
    - `ALLOW-FROM uri` — deprecated/limited browser support.
2. **CSP frame-ancestors directive** (preferred modern method)
    - `Content-Security-Policy: frame-ancestors 'self' https://trusted.com;`
    - More flexible and robust than X-Frame-Options.
3. **Frame-busting JS** (not reliable alone)
    - `if (window.top !== window.self) window.top.location = window.location;` — easily bypassed and should not be primary defense.
4. **Click-confirmations / re-auth for sensitive actions**
    - Show explicit modal, re-prompt password, 2FA for critical operations.
5. **UI hardening**
    - Use visible, user-initiated controls, add timeouts, and require gestures that are hard to simulate via clickjacking.
6. **Avoid silent privileged actions on GET requests**
    - Keep state-changing operations POST and require CSRF protections (prevents combination attacks).

### Implementation notes

- Use **CSP `frame-ancestors`** for modern browsers; fall back to `X-Frame-Options` for legacy.
- Test across browsers — behavior and header support differs.
- Do not rely solely on JavaScript frame-busting.

### Detection / indicators

- Pages missing frame protection headers.
- Sensitive actions performed without explicit confirmation.
- Unusual UI overlays reported from user sessions.

---

##