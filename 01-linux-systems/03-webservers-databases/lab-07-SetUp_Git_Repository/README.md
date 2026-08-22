# Lab 07: Set Up Git Repository on Storage Server

## Objective

Learn how to set up a centralized Git repository on a storage server, allowing developers and DevOps engineers to push code directly to a server without relying on external platforms such as GitHub, GitLab, or Bitbucket.

By the end of this lab, you will be able to:

- Understand Git server architecture
- Create a bare Git repository
- Configure remote repositories
- Push code to a server
- Understand GitOps-style workflows
- Automate deployments using Git Hooks
- Troubleshoot Git connectivity issues

---

# What is a Git Repository Server?

A Git repository server is a centralized location where code is stored and shared among team members.

Example:

```text
Developer Laptop
        ↓
Central Git Repository
        ↓
Production Server
```

Instead of sharing ZIP files:

```text
Version Controlled
Traceable
Rollback Capable
Secure
```

---

# Why Do We Need a Git Server?

Benefits:

✅ Version Control

✅ Collaboration

✅ Backup of Source Code

✅ Deployment Automation

✅ Release Management

✅ Infrastructure as Code

---

# Traditional Workflow

```text
Developer
      ↓
GitHub / GitLab
      ↓
Build Pipeline
      ↓
Production
```

---

# Private Git Workflow

Sometimes organizations prefer:

```text
Developer
      ↓
Storage Server
      ↓
Production Deployment
```

Reasons:

- Security
- Compliance
- Internal Projects
- Air-Gapped Environments

---

# What is a Bare Repository?

There are two types of Git repositories.

---

## Normal Repository

Contains:

```text
Working Directory
Source Files
.git Directory
```

Example:

```text
my-app/
│
├── index.html
├── app.js
└── .git
```

Used by developers.

---

## Bare Repository

Contains:

```text
Only Git Metadata
No Working Files
```

Example:

```text
my_project.git/
```

Contents:

```text
HEAD
objects/
refs/
hooks/
```

No source code files are checked out.

---

# Why Use Bare Repositories?

Because multiple users can safely:

```text
Push
Pull
Clone
```

from a central location.

---

# Architecture

```text
Developer Laptop
          ↓
       git push
          ↓
Storage Server
(Bare Repository)
          ↓
Deployment Target
```

---

# Creating the Bare Repository

Login to storage server.

Create repository:

```bash
git init --bare my_project.git
```

Expected output:

```text
Initialized empty Git repository
```

---

# Understanding the Command

```bash
git init
```

Creates a Git repository.

---

```bash
--bare
```

Creates:

```text
Central Repository
No Working Directory
```

---

# Verify Repository

Check contents:

```bash
ls -l my_project.git
```

Example:

```text
HEAD
objects
refs
hooks
config
```

---

# Repository Structure

```text
my_project.git
│
├── HEAD
├── config
├── hooks
├── refs
└── objects
```

---

# Configure Local Git Repository

On developer laptop:

```bash
cd my_project
```

Verify local repository:

```bash
git status
```

---

# Add Remote Repository

```bash
git remote add production user@server:/path/to/my_project.git
```

Example:

```bash
git remote add production devops@10.0.0.20:/repositories/my_project.git
```

---

# Verify Remote

```bash
git remote -v
```

Output:

```text
production
```

with repository location.

---

# Push Code

```bash
git push production master
```

or modern Git:

```bash
git push production main
```

Expected:

```text
Writing objects...
Done.
```

---

# What Happens During a Push?

```text
Local Git Repository
          ↓
SSH Connection
          ↓
Storage Server
          ↓
Bare Repository
```

Git transfers:

```text
Commits
Branches
Tags
History
```

---

# Understanding SSH Requirement

Git uses:

```text
SSH
```

for secure communication.

Verify:

```bash
ssh user@server
```

Should connect successfully.

---

# Passwordless Authentication

Recommended for automation.

Generate SSH key:

```bash
ssh-keygen
```

Copy key:

```bash
ssh-copy-id user@server
```

---

# Verify Git Access

Test:

```bash
git ls-remote production
```

Expected:

```text
Repository References
```

---

# GitOps Concept

GitOps means:

```text
Git Becomes Source Of Truth
```

Workflow:

```text
Developer
      ↓
Commit
      ↓
Push
      ↓
Git Repository
      ↓
Deployment
```

---

# What are Git Hooks?

Git Hooks are scripts that automatically execute when certain Git events occur.

Example:

```text
Push Received
       ↓
Deploy Application
```

---

# Common Hooks

## post-receive

Runs after:

```text
git push
```

---

## pre-receive

Runs before:

```text
git push
```

---

## post-update

Runs when references are updated.

---

# Example Deployment Hook

Create:

```bash
my_project.git/hooks/post-receive
```

Example:

```bash
#!/bin/bash

git --work-tree=/var/www/html \
--git-dir=/repositories/my_project.git \
checkout -f
```

Make executable:

```bash
chmod +x hooks/post-receive
```

---

# How It Works

```text
Developer Push
      ↓
Git Hook Executes
      ↓
Code Automatically Deploys
```

No manual copy required.

---

# Production Deployment Model

```text
Developer
      ↓
Git Push
      ↓
Bare Repository
      ↓
Post-Receive Hook
      ↓
Application Directory
      ↓
Nginx/Apache
```

---

# Real-World Example

Application:

```text
inventory-app
```

Repository:

```text
inventory-app.git
```

Deployment directory:

```text
/var/www/html
```

Developer workflow:

```bash
git add .
git commit -m "Feature Update"
git push production main
```

Deployment occurs automatically.

---

# Common Troubleshooting Scenarios

---

# Scenario 1: Push Rejected

Error:

```text
Permission denied
```

---

## Investigation

Verify SSH:

```bash
ssh user@server
```

---

## Root Cause

SSH key missing.

---

## Fix

```bash
ssh-copy-id user@server
```

---

