# Bash Scripting Roadmap for a Junior DevOps Engineer

## Overview

Bash scripting is one of the most important skills for Linux Administrators and DevOps Engineers. It helps automate repetitive tasks, monitor systems, perform backups, manage users, and troubleshoot production issues.

A Junior DevOps Engineer should be comfortable with:

- Variables
- Loops (`for`, `while`)
- Conditions (`if`)
- Functions
- User Input (`read`)
- Command Substitution (`$( )`)
- File Handling
- Service Monitoring
- Basic Automation Scripts
- Log Parsing (`grep`, `awk`, `sed`)

---

# 1. Variables

Variables store values for later use.

```bash
#!/bin/bash

NAME="Asritha"

echo "Welcome $NAME"
```

Output:

```text
Welcome Asritha
```

---

# 2. User Input

Collect input from the user.

```bash
#!/bin/bash

echo "Enter your name:"
read NAME

echo "Hello $NAME"
```

---

# 3. Command Substitution

Store command output in a variable.

```bash
#!/bin/bash

TODAY=$(date)

echo "$TODAY"
```

---

# 4. If Conditions

Make decisions in scripts.

```bash
#!/bin/bash

NUMBER=20

if [ $NUMBER -gt 10 ]
then
    echo "Greater than 10"
else
    echo "Less than or equal to 10"
fi
```

---

# 5. For Loop

Execute commands repeatedly.

```bash
#!/bin/bash

for SERVER in web1 web2 web3
do
    echo "$SERVER"
done
```

Output:

```text
web1
web2
web3
```

---

# 6. While Loop

Loop until a condition becomes false.

```bash
#!/bin/bash

COUNT=1

while [ $COUNT -le 5 ]
do
    echo $COUNT
    COUNT=$((COUNT+1))
done
```

---

# 7. Functions

Reusable blocks of code.

```bash
#!/bin/bash

greet() {
    echo "Welcome to DevOps"
}

greet
```

---

# 8. File Handling

Create and write files.

```bash
#!/bin/bash

echo "Backup completed" > backup.log
```

Append:

```bash
echo "Second backup completed" >> backup.log
```

Read file:

```bash
cat backup.log
```

---

# 9. Log Parsing Using grep

Search log entries.

```bash
#!/bin/bash

grep "ERROR" /var/log/syslog
```

---

# 10. Log Parsing Using awk

Extract specific fields.

```bash
#!/bin/bash

df -h | awk '{print $1,$5}'
```

---

# 11. Log Parsing Using sed

Modify text.

```bash
#!/bin/bash

echo "95%" | sed 's/%//g'
```

Output:

```text
95
```

---

# Essential DevOps Practice Scripts

---

# Script 1: Disk Monitoring

## Objective

Alert when disk usage exceeds a threshold.

```bash
#!/bin/bash

THRESHOLD=80

USAGE=$(df / | awk 'NR==2 {gsub("%",""); print $5}')

if [ $USAGE -gt $THRESHOLD ]
then
    echo "Disk usage critical: ${USAGE}%"
else
    echo "Disk usage normal: ${USAGE}%"
fi
```

Sample Output:

```text
Disk usage critical: 85%
```

---

# Script 2: Service Monitoring

## Objective

Check whether Nginx is running.

```bash
#!/bin/bash

STATUS=$(systemctl is-active nginx)

if [ "$STATUS" = "active" ]
then
    echo "Nginx is running"
else
    echo "Nginx is down"
fi
```

Output:

```text
Nginx is running
```

---

# Script 3: Auto-Restart Service

## Objective

Restart Nginx automatically if it goes down.

```bash
#!/bin/bash

STATUS=$(systemctl is-active nginx)

if [ "$STATUS" != "active" ]
then
    systemctl restart nginx
    echo "Nginx restarted"
else
    echo "Nginx already running"
fi
```

---

# Script 4: Website Backup Script

## Objective

Create a compressed backup of website files.

```bash
#!/bin/bash

BACKUP_FILE="/tmp/website_backup.tar.gz"

tar -czf $BACKUP_FILE /var/www/html

echo "Backup created at $BACKUP_FILE"
```

Output:

```text
Backup created at /tmp/website_backup.tar.gz
```

---

# Script 5: Website ZIP Backup

```bash
#!/bin/bash

zip -r website_backup.zip /var/www/html

echo "ZIP backup completed"
```

---

# Script 6: Database Backup

## Objective

Backup a MariaDB/MySQL database.

```bash
#!/bin/bash

DB_NAME="mydb"
BACKUP_FILE="/tmp/mydb.sql"

mysqldump -u root -p $DB_NAME > $BACKUP_FILE

echo "Database backup completed"
```

---

# Script 7: User Creation

## Objective

Create a new Linux user.

```bash
#!/bin/bash

echo "Enter username:"
read USERNAME

useradd $USERNAME

echo "$USERNAME created successfully"
```

---

# Script 8: Multiple User Creation

```bash
#!/bin/bash

for USER in dev1 dev2 dev3
do
    useradd $USER
    echo "$USER created"
done
```

---

# Script 9: User Exists Check

```bash
#!/bin/bash

echo "Enter username:"
read USERNAME

if id "$USERNAME" &>/dev/null
then
    echo "User exists"
else
    echo "User does not exist"
fi
```

---

# Script 10: Server Health Check

## Objective

View overall server status.

```bash
#!/bin/bash

echo "================================"
echo "SERVER HEALTH REPORT"
echo "================================"

echo ""
echo "Hostname:"
hostname

echo ""
echo "Uptime:"
uptime

echo ""
echo "Disk Usage:"
df -h

echo ""
echo "Memory Usage:"
free -h
```

---

# Script 11: Advanced Server Health Check

```bash
#!/bin/bash

echo "Hostname:"
hostname

echo ""
echo "Date:"
date

echo ""
echo "Uptime:"
uptime

echo ""
echo "CPU Usage:"
top -bn1 | head -5

echo ""
echo "Memory:"
free -h

echo ""
echo "Disk:"
df -h

echo ""
echo "Nginx Status:"
systemctl is-active nginx

echo ""
echo "MariaDB Status:"
systemctl is-active mariadb
```

---

# Script 12: Process Monitor

## Objective

Display top CPU-consuming processes.

```bash
#!/bin/bash

ps aux --sort=-%cpu | head
```

---

# Script 13: Memory Monitor

```bash
#!/bin/bash

free -h
```

---

# Script 14: SSH Connectivity Check

```bash
#!/bin/bash

ssh user@server "hostname"
```

Output:

```text
webserver01
```

---

# Script 15: Port Check Script

## Objective

Check if a service is listening on port 80.

```bash
#!/bin/bash

ss -tulpn | grep :80
```

---

# Script 16: Website Availability Check

```bash
#!/bin/bash

curl -I http://localhost
```

Expected:

```text
HTTP/1.1 200 OK
```

---

# Script 17: Log Cleanup Script

## Objective

Remove logs older than 30 days.

```bash
#!/bin/bash

find /var/log -type f -mtime +30 -delete

echo "Old logs deleted"
```

---

# Script 18: Directory Archive Script

```bash
#!/bin/bash

tar -czf archive.tar.gz /data

echo "Archive completed"
```

---

# Script 19: Complete Production Health Check

## Objective

A script commonly asked in interviews.

```bash
#!/bin/bash

echo "================================"
echo "PRODUCTION HEALTH CHECK"
echo "================================"

echo ""
echo "Hostname:"
hostname

echo ""
echo "Current Time:"
date

echo ""
echo "Uptime:"
uptime

echo ""
echo "Disk Usage:"
df -h

echo ""
echo "Memory Usage:"
free -h

echo ""
echo "Top CPU Processes:"
ps aux --sort=-%cpu | head

echo ""
echo "Open Ports:"
ss -tulpn

echo ""
echo "Nginx Status:"
systemctl is-active nginx

echo ""
echo "MariaDB Status:"
systemctl is-active mariadb
```

---

# Interview Question

## How Good Should Bash Be for a Junior DevOps Engineer?

A Junior DevOps Engineer should be comfortable with:

### Core Bash Skills

✅ Variables

✅ Loops (`for`, `while`)

✅ Conditions (`if`, `else`)

✅ Functions

✅ User Input (`read`)

✅ Command Substitution (`$( )`)

✅ File Handling

✅ Log Parsing (`grep`, `awk`, `sed`)

✅ Process Handling

✅ Cron-based automation

### Administration Skills

✅ Service Monitoring

✅ User Management

✅ Process Monitoring

✅ Log Analysis

✅ Health Checks

✅ Backups

✅ Archive Management

### Automation Skills

✅ Scheduled Backups

✅ Service Auto-Restarts

✅ Disk Monitoring

✅ Website Monitoring

✅ SSH-based automation

✅ Basic deployment scripts

---

# Must-Know Interview Scripts

If you can confidently write these without copying:

1. Disk Monitoring Script
2. Service Status Checker
3. Auto Service Restart Script
4. User Creation Script
5. Server Health Check Script
6. Backup Script
7. Database Backup Script
8. Process Monitoring Script
9. Log Cleanup Script
10. Website Availability Script

You are already ahead of many entry-level Linux Administrators and Junior DevOps Engineers.

---

# Learning Progression

```text
Linux Commands
        ↓
Bash Basics
        ↓
Conditional Logic
        ↓
Loops & Functions
        ↓
File Handling
        ↓
System Monitoring
        ↓
Automation Scripts
        ↓
Cron Jobs
        ↓
Ansible
        ↓
DevOps Automation
```

The goal is to understand how to automate repetitive Linux administration tasks and solve real production problems using Bash.
