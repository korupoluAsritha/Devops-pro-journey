# Linux Administration Scenarios & Troubleshooting Guide

## Purpose

This document contains real-world Linux administration scenarios that frequently occur in production environments.

Focus Areas:

- User Management
- SSH
- Permissions
- Services
- Cron Jobs
- SELinux
- Script Execution
- System Access

---

# Linux Troubleshooting Mindset

Before fixing anything:

```text
Observe
   ↓
Verify
   ↓
Collect Evidence
   ↓
Identify Root Cause
   ↓
Implement Fix
   ↓
Verify Resolution
```

Never guess.

Always verify.

---

# Scenario 1: User Cannot Login

## Problem

User reports:

```text
I cannot SSH into the server.
```

---

## Troubleshooting Flow

```mermaid
flowchart TD
    A["User Cannot Login"]
    B["Does User Exist?"]
    C["Check Account Expiry"]
    D["Check Password Status"]
    E["Check User Shell"]
    F["Check SSH Logs"]
    G["Root Cause Identified"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
```

---

## Step 1

Check user:

```bash
id username
```

---

## Step 2

Check expiry:

```bash
chage -l username
```

---

## Step 3

Check password:

```bash
passwd -S username
```

---

## Step 4

Check shell:

```bash
grep username /etc/passwd
```

Verify:

```text
/bin/bash
```

or

```text
/sbin/nologin
```

---

## Step 5

Check SSH logs:

```bash
journalctl -u sshd
```

---

# Scenario 2: Jenkins Service Failed

## Problem

Jenkins UI inaccessible.

---

## Troubleshooting Flow

```mermaid
flowchart TD
    A["Jenkins Not Accessible"]
    B["Service Running?"]
    C["Check Logs"]
    D["Check Port"]
    E["Check File Permissions"]
    F["Restart Service"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

---

## Verify Service

```bash
systemctl status jenkins
```

---

## Check Logs

```bash
journalctl -u jenkins
```

---

## Check Listening Port

```bash
ss -tulpn | grep 8080
```

---

## Verify Ownership

```bash
ls -ld /var/lib/jenkins
```

Expected:

```text
jenkins jenkins
```

---

# Scenario 3: Nginx Website Returns 403

## Problem

Website accessible but returns:

```text
403 Forbidden
```

---

## Troubleshooting Flow

```mermaid
flowchart TD
    A["403 Error"]
    B["Check File Permissions"]
    C["Check Ownership"]
    D["Check SELinux"]
    E["Verify Nginx Config"]
    F["Issue Resolved"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

---

## Check Permissions

```bash
ls -l /var/www/html
```

---

## Check SELinux

```bash
sestatus
```

For testing:

```bash
setenforce 0
```

If website works:

```text
SELinux likely caused issue
```

---

# Scenario 4: Script Gives Permission Denied

## Problem

```bash
./backup.sh
```

Output:

```text
Permission denied
```

---

## Troubleshooting Flow

```mermaid
flowchart TD
    A["Script Fails"]
    B["Check Execute Permission"]
    C["Check Ownership"]
    D["Check Shebang"]
    E["Check SELinux"]
    F["Execute Successfully"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

---

## Verify Permission

```bash
ls -l backup.sh
```

Missing:

```text
x
```

---

## Fix

```bash
chmod +x backup.sh
```

---

## Verify Shebang

```bash
#!/bin/bash
```

---

# Scenario 5: SSH Key Authentication Fails

## Problem

```bash
Permission denied (publickey)
```

---

## Troubleshooting Flow

```mermaid
flowchart TD
    A["SSH Key Fails"]
    B["Check Private Key"]
    C["Check authorized_keys"]
    D["Check Permissions"]
    E["Check SSH Config"]
    F["Authentication Success"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

---

## Verify SSH Directory

```bash
ls -ld ~/.ssh
```

Expected:

```text
700
```

---

## Verify Authorized Keys

```bash
ls -l ~/.ssh/authorized_keys
```

Expected:

```text
600
```

---

## Debug SSH

```bash
ssh -v user@server
```

---

# Scenario 6: Cron Job Not Running

## Problem

Expected backup did not execute.

---

## Troubleshooting Flow

```mermaid
flowchart TD
    A["Cron Job Failed"]
    B["Cron Service Running?"]
    C["Check Crontab"]
    D["Check Script"]
    E["Check Logs"]
    F["Issue Resolved"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

---

## Verify Cron Service

```bash
systemctl status crond
```

---

## View Jobs

```bash
crontab -l
```

---

## Run Script Manually

```bash
./backup.sh
```

---

## Review Logs

```bash
cat /var/log/cron_output.log
```

---

# Scenario 7: User Suddenly Lost Access

## Problem

Contractor says:

```text
Login worked yesterday but fails today.
```

---

## Investigation

Check:

```bash
chage -l contractor
```

Possible result:

```text
Account expires: Aug 07, 2026
```

Today's date:

```text
Aug 08, 2026
```

Root Cause:

```text
Account Expired
```

---

## Fix

```bash
sudo usermod -e 2026-12-31 contractor
```

---

# Scenario 8: Root Login No Longer Works

## Problem

Admin cannot login using:

```bash
ssh root@server
```

---

## Investigation

Check:

```bash
cat /etc/ssh/sshd_config
```

Find:

```text
PermitRootLogin no
```

This is expected.

---

## Recommended Access

```bash
ssh adminuser@server
```

Then:

```bash
sudo -i
```

---

# Scenario 9: Server Running Slow

## Symptoms

```text
High CPU
Slow Response
Application Delay
```

---

## Troubleshooting Flow

```mermaid
flowchart TD
    A["Server Slow"]
    B["Check CPU"]
    C["Check Memory"]
    D["Check Disk"]
    E["Check Processes"]
    F["Identify Bottleneck"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```

---

## CPU

```bash
top
```

or

```bash
htop
```

---

## Memory

```bash
free -h
```

---

## Disk

```bash
df -h
```

---

## Processes

```bash
ps aux --sort=-%cpu
```

---

# Scenario 10: Disk Full

## Symptoms

```text
Application Crashes
Unable To Write Files
Database Errors
```

---

## Troubleshooting Flow

```mermaid
flowchart TD
    A["Disk Full"]
    B["Check Disk Usage"]
    C["Find Large Files"]
    D["Remove Logs"]
    E["Verify Space"]

    A --> B
    B --> C
    C --> D
    D --> E
```

---

## Check Disk Space

```bash
df -h
```

---

## Find Large Directories

```bash
du -sh /*
```

---

## Find Large Files

```bash
find / -type f -size +500M
```

---

# Golden Rules of Linux Troubleshooting

## Rule 1

Never change configuration before taking a backup.

```bash
cp file.conf file.conf.bak
```

---

## Rule 2

Read logs first.

```bash
journalctl
```

```bash
/var/log/*
```

---

## Rule 3

Verify assumptions.

Do not assume:

```text
Service is running
```

Check:

```bash
systemctl status service
```

---

## Rule 4

Test one change at a time.

Avoid changing multiple settings simultaneously.

---

## Rule 5

Always validate fixes.

Example:

```bash
sshd -t
```

before restarting SSH.

---

# DevOps Troubleshooting Formula

Whenever something fails:

```text
Service
   ↓
Logs
   ↓
Permissions
   ↓
Network
   ↓
Authentication
   ↓
Security Controls
   ↓
Root Cause
```

This simple framework works for Linux, Docker, Kubernetes, Jenkins, Terraform, and cloud platforms.
