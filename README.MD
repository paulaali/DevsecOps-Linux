<html>
<head></head>
<body>
<h1 align="center">🛡️ DevSecOps Practical Assessment</h1>
<h2 align="center">Linux User & Access Control</h2>

<p align="center">
<b>Author:</b> Your Name <br>
<b>Role:</b> DevSecOps Engineer <br>
<b>Date:</b> 21/02/2026
</p>

<hr>

<h2>📌 Overview</h2>

<p>
This repository documents the execution of a scenario-based Linux security assessment focused on:
</p>

<ul>
<li>Password state investigation & remediation</li>
<li>User and department-based access control</li>
<li>Least privilege enforcement</li>
<li>Service account hardening</li>
<li>Privilege escalation control</li>
</ul>

<p>
All tasks were performed entirely in the Linux terminal and validated with system-level verification.
</p>

<hr>

<h1>🔎 SCENARIO 1 – Password State Investigation & Remediation</h1>

<h2>Part A – Create a User Without a Password</h2>

<h3>Step 1: Create User</h3>

```bash
sudo useradd audit_test
```

<h3>Step 2: Confirm User Exists</h3>

```bash
id audit_test
grep audit_test /etc/passwd
```

<h3>Observation</h3>

- The user entry exists in `/etc/passwd`
- The password field contains `x`
- This indicates password data is stored elsewhere

<h3>Explanation</h3>

Linux stores:
- User identity information → `/etc/passwd`
- Password hashes → `/etc/shadow`

This separation improves system security.

---

<h2>Part B – Identify Users Without Passwords</h2>

<h3>Inspect Password Storage</h3>

```bash
sudo cat /etc/shadow
```

<h3>Finding</h3>

Example output:

```
audit_test:!:19876:0:99999:7:::
```

<h3>Meaning of Symbols</h3>

| Symbol | Meaning |
|--------|---------|
| `!` | Account locked / no active password |
| `*` | Login disabled |
| Empty | No password set |
| `$6$` | SHA-512 hashed password |

<h3>Security Validation</h3>

Test as non-privileged user:

```bash
su - audit_test
cat /etc/shadow
```

Result:

```
Permission denied
```

<h3>Conclusion</h3>

- `/etc/shadow` is root-protected
- Password hashes are secured
- Non-privileged users cannot access authentication data

---

<h2>Part C – Remediation</h2>

<h3>Set Password</h3>

```bash
sudo passwd audit_test
```

<h3>Verify Password Hash</h3>

```bash
sudo grep audit_test /etc/shadow
```

After remediation:

```
audit_test:$6$randomhashvalue...
```

<h3>Before vs After</h3>

| Before | After |
|--------|--------|
| `!` | `$6$hashvalue` |
| No authentication | Secure hashed password |

<h3>Security Insight</h3>

Linux separates:
- Identity management
- Authentication storage

This layered architecture strengthens system security.

<hr>

<h1>🏢 SCENARIO 2 – DevSecOps User & Department Access Configuration</h1>

<h2>Create Department Groups</h2>

```bash
sudo groupadd dev_team
sudo groupadd sec_team
sudo groupadd qa_team
sudo groupadd ops_team
```

Group data is stored in:

```
/etc/group
```

---

<h2>Create Users</h2>

```bash
sudo useradd -m lateef
sudo adduser purity
sudo useradd -m arthur
sudo useradd -m paula
sudo useradd -m yaa
sudo useradd -m alex
sudo useradd -m habiba
sudo useradd -m aminat
```

---

<h2>Assign Department Membership</h2>

```bash
sudo usermod -aG dev_team lateef
sudo usermod -aG dev_team arthur

sudo usermod -aG sec_team purity
sudo usermod -aG sec_team habiba

sudo usermod -aG qa_team paula
sudo usermod -aG qa_team yaa

sudo usermod -aG ops_team alex
sudo usermod -aG ops_team aminat
```

Verify:

```bash
groups alex
```

---

<h2>Create Service Account (Non-Login)</h2>

```bash
sudo useradd -r -s /usr/sbin/nologin ci_runner
```

<h3>Why Non-Login?</h3>

<ul>
<li>Prevents interactive access</li>
<li>Reduces attack surface</li>
<li>Enforces least privilege</li>
<li>Aligns with DevSecOps security standards</li>
</ul>

---

<h2>Grant Sudo Access to ONE ops_team Member</h2>

```bash
sudo usermod -aG sudo alex
```

Verification:

```bash
su - alex
sudo whoami
```

Expected output:

```
root
```

All other users remain restricted.

---

<h2>Remove User but Preserve Home Directory</h2>

```bash
sudo userdel purity
```

Home directory remains intact.

<h3>Why Preserve?</h3>

<ul>
<li>Audit traceability</li>
<li>Forensic investigations</li>
<li>Compliance requirements</li>
<li>Data retention policy</li>
</ul>

---

<h1>🔐 Technical Deep Dive</h1>

<h2>How Linux Tracks Users</h2>

| File | Purpose |
|------|----------|
| `/etc/passwd` | User identity data |
| `/etc/shadow` | Password hashes |
| `/etc/group` | Group membership |
| `/etc/sudoers` | Privilege escalation control |

---

<h2>How Department-Based Access Control Works</h2>

Linux uses:

- Owner
- Group
- Others

Example:

```bash
chown :dev_team project/
chmod 770 project/
```

Only `dev_team` members can access.

---

<h2>Privilege Escalation Control</h2>

- Only selected users added to `sudo`
- Sudo activity logged in:
  ```
  /var/log/auth.log
  ```
- Principle of Least Privilege enforced

---

<h1>📊 Key DevSecOps Principles Demonstrated</h1>

<ul>
<li>Least Privilege</li>
<li>Role-Based Access Control (RBAC)</li>
<li>Password State Auditing</li>
<li>Service Account Hardening</li>
<li>Controlled Privilege Escalation</li>
<li>Audit Preservation</li>
</ul>

---

<h1>🧠 Professional Reflection</h1>

<p>
This assessment simulated real-world DevSecOps onboarding and security auditing.
</p>

<p>
Through direct interaction with Linux authentication files, group configurations, and privilege boundaries, I demonstrated:
</p>

<ul>
<li>System-level security awareness</li>
<li>Operational hardening practices</li>
<li>Structured access control implementation</li>
<li>Audit-ready documentation</li>
</ul>

<p>
This project reflects practical Linux security administration aligned with enterprise DevSecOps standards.
</p>

---

<h3 align="center">🚀 End of Assessment</h3>
</body>
</html>
