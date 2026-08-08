# Linux Interview Questions

## Beginner Level

### What is Linux?

Linux is an open-source, multi-user, multitasking operating system commonly used in servers and cloud environments.

---

### What is the root user?

The root user is the superuser with unrestricted access to the system.

---

### What is a shell?

A shell is a command interpreter that allows users to interact with the Linux kernel.

Examples:

```text
bash
sh
zsh
```

---

### Difference Between Kernel and Shell?

Kernel:

```text
Core of the OS
```

Shell:

```text
Interface between user and kernel
```

---

### What Are Linux File Permissions?

```text
r = Read
w = Write
x = Execute
```

---

### What Does chmod Do?

Changes file permissions.

Example:

```bash
chmod +x script.sh
```

---

### What Does chown Do?

Changes file ownership.

Example:

```bash
chown user:group file.txt
```

---

## User Management

### Difference Between Normal User and Service User?

Normal User:

```text
Human login account
```

Service User:

```text
Application account
```

Examples:

```text
nginx
mysql
jenkins
```

---

### Why Use /sbin/nologin?

To prevent interactive login while allowing the account to own files and run services.

---

### How Do You Verify a User's Shell?

```bash
grep username /etc/passwd
```

---

### How Do You Create a Temporary User?

```bash
sudo useradd -e YYYY-MM-DD username
```

---

### How Do You Check Account Expiry?

```bash
chage -l username
```

---

## SSH

### What is SSH?

Secure Shell used for encrypted remote access.

---

### What Port Does SSH Use?

```text
22
```

by default.

---

### Why Disable Root SSH Login?

To reduce attack surface and improve auditing.

---

### Which File Controls SSH Server Configuration?

```text
/etc/ssh/sshd_config
```

---

### What Does PermitRootLogin No Mean?

Direct root login over SSH is prohibited.

---

### How Do You Check SSH Configuration Before Restarting?

```bash
sshd -t
```

---

## SSH Keys

### Difference Between Public Key and Private Key?

Public Key:

```text
Shared with servers
```

Private Key:

```text
Kept secret
```

---

### Which Command Creates SSH Keys?

```bash
ssh-keygen -t rsa
```

---

### Which Command Copies a Public Key?

```bash
ssh-copy-id user@server
```

---

### Where Are Authorized Keys Stored?

```text
~/.ssh/authorized_keys
```

---

### Why Is SSH Key Authentication Preferred?

- More secure
- No password prompts
- Supports automation

---

## Cron

### What Is Cron?

Linux task scheduler.

---

### How Do You Create a Cron Job?

```bash
crontab -e
```

---

### What Does This Mean?

```bash
0 0 * * *
```

Run daily at midnight.

---

### Why Use Absolute Paths In Cron?

Cron runs with a minimal environment and limited PATH variables.

---

### How Do You Check Existing Cron Jobs?

```bash
crontab -l
```

---

## SELinux

### What Is SELinux?

A Linux security framework providing Mandatory Access Control.

---

### What Problem Does SELinux Solve?

Limits what processes can access even if normal permissions allow it.

---

### What Are SELinux Modes?

```text
Enforcing
Permissive
Disabled
```

---

### Which Command Shows SELinux Status?

```bash
sestatus
```

---

### Which Command Shows Current SELinux Mode?

```bash
getenforce
```

---

### How Do You Enable Enforcing Mode?

```bash
setenforce 1
```

---

## Scenario Based Questions

### A Script Shows Permission Denied. What Would You Check?

1. File permissions
2. Ownership
3. Execute bit
4. SELinux restrictions

Verify:

```bash
ls -l script.sh
```

---

### A User Cannot Login. What Would You Check?

1. Account expiry

```bash
chage -l username
```

2. Shell

```bash
grep username /etc/passwd
```

3. Password status

```bash
passwd -S username
```

4. SSH logs

```bash
journalctl -u sshd
```

---

### A Cron Job Is Not Running. How Do You Troubleshoot?

1. Check cron service

```bash
systemctl status crond
```

2. Verify job

```bash
crontab -l
```

3. Review logs
4. Check paths
5. Test script manually

---

### A Service Runs As Jenkins User. How Do You Troubleshoot It If Jenkins Uses /sbin/nologin?

1. Check service status

```bash
systemctl status jenkins
```

2. Check logs

```bash
journalctl -u jenkins
```

3. Check permissions

```bash
ls -ld /var/lib/jenkins
```

4. Execute commands as the user

```bash
sudo -u jenkins command
```

---

## 30-Second DevOps Answer

Linux fundamentals for DevOps include:

- User and permission management
- SSH authentication
- Secure access control
- Script execution
- Cron automation
- SELinux security enforcement

These concepts form the foundation for Jenkins, Docker, Kubernetes, Ansible, Terraform, and cloud administration.
