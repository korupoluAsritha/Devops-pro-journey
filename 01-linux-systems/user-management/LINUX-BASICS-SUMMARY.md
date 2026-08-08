# Linux Basics Summary

## Overview

Linux is a multi-user, multitasking operating system widely used in servers, cloud platforms, DevOps, containers, and enterprise environments.

Everything in Linux is treated as a file.

---

# Users and Access Management

Linux supports multiple users on the same system.

Types of users:

### Root User

The superuser with unrestricted access.

```bash
whoami
```

Output:

```text
root
```

---

### Normal Users

Used by humans for daily activities.

Example:

```bash
sudo useradd -m -s /bin/bash john
```

---

### Service Users

Used by applications and services.

Examples:

```text
nginx
mysql
jenkins
postgres
```

Typically configured with:

```bash
/sbin/nologin
```

to prevent interactive login.

---

# Shell

A shell is an interface between the user and the Linux kernel.

Common shells:

```text
bash
sh
zsh
ksh
```

Architecture:

User → Shell → Kernel → Hardware

---

# Interactive vs Non-Interactive Shell

### Interactive Shell

```bash
/bin/bash
```

Allows login and command execution.

---

### Non-Interactive Shell

```bash
/sbin/nologin
```

Prevents direct login but allows service execution.

---

# Temporary Users

Used for:

- Contractors
- Auditors
- Interns
- Vendors

Create:

```bash
sudo useradd -e YYYY-MM-DD username
```

Verify:

```bash
chage -l username
```

Benefits:

- Automatic access revocation
- Improved security
- Reduced administrative effort

---

# SSH Security

SSH stands for Secure Shell.

Used for:

- Remote login
- Server administration
- Automation

Example:

```bash
ssh user@server
```

---

## Why Disable Root SSH Login?

Root is a common attack target.

Disable root login:

```text
PermitRootLogin no
```

Benefits:

- Better security
- Better auditing
- Reduced brute-force attacks

---

# SSH Key Authentication

SSH can use:

### Password Authentication

```text
Username + Password
```

### Key Authentication

```text
Public Key
Private Key
```

Generate keys:

```bash
ssh-keygen -t rsa
```

Install on remote server:

```bash
ssh-copy-id user@server
```

Benefits:

- More secure
- Passwordless automation
- Required by DevOps tools

---

# File Permissions

Linux permissions are:

```text
r = Read
w = Write
x = Execute
```

Example:

```text
-rwxr-xr-x
```

Owner:

```text
rwx
```

Group:

```text
r-x
```

Others:

```text
r-x
```

---

# Script Execution Permissions

Linux does not execute files automatically.

Make executable:

```bash
chmod +x script.sh
```

Verify:

```bash
ls -l script.sh
```

Run:

```bash
./script.sh
```

---

# Shebang

The first line of a script.

Example:

```bash
#!/bin/bash
```

Specifies which interpreter should execute the script.

---

# Cron Jobs

Cron is a Linux scheduler.

Used for:

- Backups
- Monitoring
- Cleanup
- Automation

Edit schedule:

```bash
crontab -e
```

Example:

```bash
0 0 * * * backup.sh
```

Runs every day at midnight.

---

# Cron Best Practices

Use absolute paths:

```bash
/usr/bin/python3
```

Log output:

```bash
>> /var/log/output.log 2>&1
```

Verify jobs:

```bash
crontab -l
```

---

# SELinux

SELinux stands for Security-Enhanced Linux.

Provides:

```text
Mandatory Access Control (MAC)
```

---

# SELinux Modes

### Enforcing

```text
Block + Log
```

---

### Permissive

```text
Allow + Log
```

---

### Disabled

```text
No Protection
```

---

Check Status

```bash
sestatus
```

Current Mode:

```bash
getenforce
```

Enable Enforcing:

```bash
sudo setenforce 1
```

---

# Linux Security Journey

User
↓
Authentication
↓
SSH Access
↓
Permissions
↓
Scripts
↓
Automation (Cron)
↓
Advanced Security (SELinux)

This forms the foundation of Linux administration and DevOps.
