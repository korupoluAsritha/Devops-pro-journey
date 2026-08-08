# Lab 07: Linux SSH Authentication (Passwordless Login)

## Objective

Configure SSH key-based authentication to enable secure and passwordless login between Linux systems.

By the end of this lab, you will be able to:

- Understand how SSH authentication works
- Generate SSH key pairs
- Configure passwordless login
- Understand public and private keys
- Troubleshoot SSH authentication failures
- Understand why SSH keys are critical in DevOps automation

---

# What is SSH?

SSH stands for:

```text
Secure Shell
```

SSH is a secure protocol used to connect to remote Linux servers.

Example:

```bash
ssh username@server-ip
```

SSH allows administrators to:

- Access remote servers
- Run commands
- Transfer files securely
- Automate server management

---

# Why Do We Need SSH Authentication?

Imagine a DevOps environment:

```text
Jenkins
   ↓
Deploy Application
   ↓
Remote Server
```

If Jenkins must type a password every time:

```text
Deployment Stops
Waiting For Password
```

Automation becomes impossible.

Instead:

```text
Jenkins
   ↓
SSH Key Authentication
   ↓
Remote Server
   ↓
Access Granted
```

No password required.

This is the foundation of:

- Ansible
- Jenkins
- GitHub SSH Access
- GitLab CI/CD
- Infrastructure Automation

---

# Password Authentication vs SSH Key Authentication

## Password Authentication

```text
User
  ↓
Username + Password
  ↓
Login
```

Problems:

- Password guessing attacks
- Brute-force attacks
- Difficult to automate

---

## SSH Key Authentication

```text
Private Key
      ↓
Challenge Response
      ↓
Authentication
```

Benefits:

- More secure
- Faster
- Automation friendly
- No password typing

---

# Understanding Public-Key Cryptography

SSH uses:

```text
Public Key Cryptography
```

Two keys are generated:

## Public Key

```text
id_rsa.pub
```

This key can be shared.

Stored on:

```text
Remote Server
```

---

## Private Key

```text
id_rsa
```

This key must remain secret.

Stored on:

```text
Local Machine
```

Never share it.

---

# Authentication Flow

```text
Local Machine
     │
     │ Holds Private Key
     ▼
SSH Login Request
     ▼
Remote Server
     │
     │ Holds Public Key
     ▼
Verification
     ▼
Access Granted
```

The private key never leaves the source machine.

---

# Generate SSH Key Pair

Create a key pair:

```bash
ssh-keygen -t rsa
```

Example:

```text
Generating public/private rsa key pair.
Enter file in which to save the key:
```

Press Enter.

---

Prompt:

```text
Enter passphrase:
```

Press Enter (for automation labs).

---

Files created:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

---

# Understanding Generated Files

## Private Key

```text
~/.ssh/id_rsa
```

Example:

```text
-----BEGIN RSA PRIVATE KEY-----
```

Never share this file.

---

## Public Key

```text
~/.ssh/id_rsa.pub
```

Example:

```text
ssh-rsa AAAAB3...
```

Safe to share.

---

# Viewing the Public Key

Check:

```bash
cat ~/.ssh/id_rsa.pub
```

Example:

```text
ssh-rsa AAAAB3Nz...
```

This key should be copied to target systems.

---

# Copy Public Key to Remote Server

Use:

```bash
ssh-copy-id username@remote-server-ip
```

Example:

```bash
ssh-copy-id ubuntu@192.168.1.100
```

You'll enter the password once.

SSH automatically:

```text
Creates .ssh Directory
Creates authorized_keys File
Adds Public Key
```

---

# What Happens Behind the Scenes?

SSH copies:

```text
id_rsa.pub
```

to:

```text
~/.ssh/authorized_keys
```

on the remote server.

---

# Important SSH Files

## Local Machine

```text
~/.ssh/id_rsa
```

Private key.

---

```text
~/.ssh/id_rsa.pub
```

Public key.

---

## Remote Machine

```text
~/.ssh/authorized_keys
```

Contains trusted public keys.

---

# Manual Installation Method

If `ssh-copy-id` is unavailable.

Copy key manually:

```bash
cat ~/.ssh/id_rsa.pub
```

Copy the output.

---

On remote server:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Create:

```bash
vi ~/.ssh/authorized_keys
```

Paste the public key.

Set permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

# Required Permissions

SSH is very strict about permissions.

### SSH Directory

```bash
chmod 700 ~/.ssh
```

Meaning:

```text
Owner:
  Read
  Write
  Execute

Others:
  No Access
```

---

### Authorized Keys File

```bash
chmod 600 ~/.ssh/authorized_keys
```

Meaning:

```text
Owner:
  Read
  Write

Others:
  No Access
```

---

# Testing Passwordless Login

Attempt login:

```bash
ssh username@remote-server-ip
```

Expected:

```text
Login Successful
```

No password prompt should appear.

---

# Verify Authentication Method

Use verbose mode:

```bash
ssh -v username@remote-server-ip
```

Look for:

```text
Offering public key
Authentication succeeded
```

---

# Why Is SSH Authentication Critical In DevOps?

Most DevOps tools rely on SSH.

---

## Ansible

```text
Control Node
      ↓
SSH
      ↓
Managed Servers
```

Ansible does not require an agent.

It communicates directly through SSH.

---

## Jenkins

```text
Jenkins
    ↓
SSH
    ↓
Deployment Server
```

Deployments occur automatically.

---

## GitHub

You already used:

```bash
ssh -T git@github.com
```

That is SSH key authentication.

---

## Backup Servers

```text
Server A
   ↓
SSH
   ↓
Server B
```

Passwordless backups.

---

# Troubleshooting SSH Authentication

---

## Issue 1: Password Still Requested

Check:

```bash
ls ~/.ssh
```

Verify:

```text
id_rsa
id_rsa.pub
```

exist.

---

Check:

```bash
cat ~/.ssh/authorized_keys
```

on remote server.

Public key must be present.

---

## Issue 2: Permission Denied (publickey)

Error:

```text
Permission denied (publickey)
```

Verify:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## Issue 3: Wrong Key Being Used

Run:

```bash
ssh -v username@server
```

Look for:

```text
Offering public key
```

If not shown:

```bash
ssh-add ~/.ssh/id_rsa
```

---

## Issue 4: SSH Service Not Running

Check:

```bash
systemctl status sshd
```

Expected:

```text
active (running)
```

---

## Issue 5: Incorrect Ownership

Verify:

```bash
ls -ld ~/.ssh
```

Ownership should belong to the user.

Example:

```text
ubuntu ubuntu
```

Not:

```text
root root
```

---

# Security Best Practices

✅ Share only:

```text
id_rsa.pub
```

---

✅ Protect:

```text
id_rsa
```

---

✅ Use:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

✅ Disable root login where possible.

---

✅ Consider passphrase-protected keys for production systems.

---

# Workflow Diagram

```mermaid
flowchart TD
    A["Generate SSH Key Pair"]
    B["Private Key Stored Locally"]
    C["Public Key Copied To Remote Server"]
    D["SSH Login Attempt"]
    E["Public Key Validation"]
    F["Access Granted"]

    A --> B
    A --> C
    B --> D
    C --> E
    D --> E
    E --> F
```

---

# Command Summary

Generate key pair:

```bash
ssh-keygen -t rsa
```

View public key:

```bash
cat ~/.ssh/id_rsa.pub
```

Copy public key:

```bash
ssh-copy-id username@remote-server-ip
```

Login:

```bash
ssh username@remote-server-ip
```

Verbose debugging:

```bash
ssh -v username@remote-server-ip
```

Check SSH service:

```bash
systemctl status sshd
```

---

# Key Learnings

- SSH enables secure remote access.
- SSH key authentication is more secure than passwords.
- Public keys can be shared safely.
- Private keys must remain secret.
- `ssh-keygen` creates a key pair.
- `ssh-copy-id` installs a public key on a remote server.
- Passwordless login enables DevOps automation.
- SSH is used heavily by Ansible, Jenkins, GitHub, and deployment pipelines.
- Incorrect permissions are a common cause of SSH failures.

---

# Interview Questions

## What is SSH?

SSH (Secure Shell) is a protocol used for secure remote administration of Linux systems.

---

## Why is SSH key authentication preferred over passwords?

SSH keys are more secure, resistant to brute-force attacks, and enable automation without interactive password prompts.

---

## What is the difference between a public key and a private key?

Public Key:

```text
Can be shared.
Stored on the remote system.
```

Private Key:

```text
Must remain secret.
Stored on the local machine.
```

---

## Which command generates SSH keys?

```bash
ssh-keygen -t rsa
```

---

## Which command copies a public key to a remote server?

```bash
ssh-copy-id username@remote-server-ip
```

---

## Where does the public key get stored on the remote server?

```text
~/.ssh/authorized_keys
```

---

## What permissions should `.ssh` and `authorized_keys` have?

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

## Why is SSH considered the foundation of DevOps automation?

Tools such as Ansible, Jenkins, GitHub, deployment pipelines, and server management workflows rely on SSH-based authentication to automate tasks securely without human intervention.

---
