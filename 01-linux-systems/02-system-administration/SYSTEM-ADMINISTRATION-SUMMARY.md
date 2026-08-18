# System Administration Module Summary

## Overview

System Administration focuses on managing, maintaining, securing, troubleshooting, and automating Linux servers in production environments.

This module builds upon Linux Basics and introduces real-world administration concepts such as:

- Configuration Management
- Service Troubleshooting
- Bash Scripting
- Application Hosting
- Network Troubleshooting
- Firewall Management
- Process Management

---

# Learning Journey

```text
Linux Basics
     ↓
SSH
Users
Permissions
Cron
SELinux
     ↓
System Administration
     ↓
Ansible
Database Troubleshooting
Bash Automation
Tomcat Administration
Network Services
Firewall Configuration
Process Troubleshooting
```

---

# Module 1: Ansible Installation and Configuration

## What is Ansible?

Ansible is an open-source automation and configuration management tool.

Uses:

- Server Management
- Software Installation
- Configuration Management
- Application Deployment
- Infrastructure Automation

---

## Architecture

```text
Control Node
       ↓
SSH
       ↓
Managed Nodes
```

Ansible is:

```text
Agentless
```

No software is required on target servers apart from SSH and Python.

---

## Inventory

Default Inventory:

```bash
/etc/ansible/hosts
```

Example:

```ini
[web]
web01
web02

[db]
db01
```

---

## Verify Connectivity

```bash
ansible all -m ping
```

Expected:

```json
"ping": "pong"
```

---

## Key Learnings

- Infrastructure as Code (IaC)
- Agentless automation
- Inventory management
- SSH-based communication

---

# Module 2: MariaDB Troubleshooting

## Goal

Troubleshoot database startup and connectivity problems.

---

## First Commands

```bash
systemctl status mariadb
```

```bash
journalctl -u mariadb
```

```bash
tail -n 50 /var/log/mysql/error.log
```

---

## Common Failures

### Disk Full

```bash
df -h
```

---

### Permission Issues

```bash
ls -ld /var/lib/mysql
```

---

### Port Conflict

```bash
ss -tulpn | grep 3306
```

---

### Configuration Errors

Invalid settings in:

```bash
/etc/my.cnf
```

---

## Troubleshooting Workflow

```text
Service Status
      ↓
Logs
      ↓
Root Cause
      ↓
Fix
      ↓
Verification
```

---

# Module 3: Bash Scripting

## What is Bash?

Bash stands for:

```text
Bourne Again Shell
```

Most common Linux shell.

---

## Why Use Scripts?

Automation.

Examples:

- Monitoring
- Backup
- Log Cleanup
- Health Checks
- Deployments

---

## Script Structure

```bash
#!/bin/bash

VARIABLE=value

if [ condition ]; then
   command
fi
```

---

## Useful Concepts

### Variables

```bash
NAME=Asritha
```

---

### Command Substitution

```bash
DATE=$(date)
```

---

### Conditions

```bash
if
then
else
fi
```

---

### Debugging

```bash
bash -x script.sh
```

---

## Key Learnings

- Automation mindset
- Reusable operational tasks
- Monitoring scripts

---

# Module 4: Apache Tomcat Administration

## What is Tomcat?

Apache Tomcat is a Java application server.

Used for:

```text
Servlets
JSP
Spring Applications
Java Web Applications
```

---

## Java Requirement

Verify:

```bash
java -version
```

---

## Installation

Extract:

```bash
tar -xvf apache-tomcat*.tar.gz
```

Start:

```bash
./bin/startup.sh
```

Stop:

```bash
./bin/shutdown.sh
```

---

## Default Port

```text
8080
```

Check:

```bash
ss -tulpn | grep 8080
```

---

## Important Logs

```bash
logs/catalina.out
```

---

## Common Issues

- JAVA_HOME not set
- Port conflicts
- Firewall restrictions
- Wrong deployment

---

# Module 5: Linux Network Services

## Goal

Identify which applications are listening on network ports.

---

## Primary Command

```bash
ss -tulpn
```

---

## Important Flags

```text
-t TCP
-u UDP
-l Listening
-p Process
-n Numeric
```

---

## Common Ports

### SSH

```text
22
```

### HTTP

```text
80
```

### HTTPS

```text
443
```

### MariaDB

```text
3306
```

### Tomcat

```text
8080
```

---

## Example

```bash
ss -tulpn | grep :80
```

Verify web server.

---

## Key Learnings

- Port verification
- Service troubleshooting
- Process-port relationships

---

# Module 6: IPTables Firewall

## Purpose

Protect Linux servers by filtering traffic.

---

## Concepts

```text
Allow
Block
Inspect
Filter
```

---

## Important Chains

### INPUT

Incoming traffic.

---

### OUTPUT

Outgoing traffic.

---

### FORWARD

Traffic passing through server.

---

## Allow HTTP

```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

---

## View Rules

```bash
iptables -L -n
```

---

## Secure Approach

```text
Allow Required Traffic
       ↓
Drop Everything Else
```

---

## Key Learnings

- Firewall basics
- Network security
- Rule ordering importance

---

# Module 7: Linux Process Troubleshooting

## What is a Process?

A running program.

Examples:

```text
nginx
mysql
java
tomcat
sshd
```

---

## Process Monitoring

### top

```bash
top
```

---

### htop

```bash
htop
```

---

### View Process

```bash
ps -ef
```

---

## Process ID (PID)

Every running process has a unique:

```text
PID
```

---

## Graceful Termination

```bash
kill -15 PID
```

Signal:

```text
SIGTERM
```

---

## Force Termination

```bash
kill -9 PID
```

Signal:

```text
SIGKILL
```

---

## Resource Monitoring

CPU:

```bash
ps aux --sort=-%cpu
```

Memory:

```bash
ps aux --sort=-%mem
```

---

## Key Learnings

- Resource analysis
- Identifying runaway processes
- Safe service recovery

---

# End-to-End Troubleshooting Framework

Whenever something fails:

```text
User Reports Issue
        ↓
Check Service Status
        ↓
Check Logs
        ↓
Check Processes
        ↓
Check Ports
        ↓
Check Firewall
        ↓
Check Permissions
        ↓
Check Resources
        ↓
Identify Root Cause
        ↓
Implement Fix
        ↓
Verify
```

---

# DevOps Perspective

This module teaches the skills required before moving into:

```text
Docker
Kubernetes
Jenkins
Terraform
Cloud Platforms
```

Core Skills Learned:

✅ Automation

✅ Troubleshooting

✅ Service Management

✅ Application Management

✅ Network Analysis

✅ Firewall Configuration

✅ Process Investigation

✅ Root Cause Analysis

These skills form the foundation of every Linux Administrator, Site Reliability Engineer (SRE), and DevOps Engineer
