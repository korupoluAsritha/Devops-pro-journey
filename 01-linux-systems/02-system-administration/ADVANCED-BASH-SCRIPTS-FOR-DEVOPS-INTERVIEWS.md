
# Advanced Bash Scripts for DevOps Interviews

## Overview

These are the most practical Bash scripts used by Linux Administrators, Site Reliability Engineers (SREs), and DevOps Engineers in real production environments.

These scripts teach:

- Monitoring
- Automation
- Backup Management
- Service Recovery
- Security Auditing
- Infrastructure Reporting
- Deployment Automation

---

# 1. Disk Monitoring Script

## Why Do We Need It?

Disk space exhaustion is one of the most common causes of:

- Database failures
- Application crashes
- Backup failures
- Log write failures

---

## Script

```bash
#!/bin/bash

THRESHOLD=80

USAGE=$(df / | awk 'NR==2 {gsub("%",""); print $5}')

if [ $USAGE -gt $THRESHOLD ]
then
    echo "WARNING: Disk Usage Critical - ${USAGE}%"
else
    echo "Disk Usage Normal - ${USAGE}%"
fi
```

---

## Explanation

```bash
df /
```

Checks disk usage.

```bash
awk
```

Extracts percentage usage.

```bash
gsub
```

Removes `%`.

---

## Expected Output

```text
Disk Usage Normal - 65%
```

or

```text
WARNING: Disk Usage Critical - 92%
```

---

## Interview Question

How would you monitor disk usage on a Linux server?

Answer:

Use `df`, store the value in a variable, compare against a threshold, and generate an alert if usage exceeds the configured limit.

---

# 2. Memory Monitoring Script

## Why Do We Need It?

High memory usage can lead to:

- Application slowness
- OOM (Out Of Memory) kills
- Server crashes

---

## Script

```bash
#!/bin/bash

MEM=$(free | awk '/Mem:/ {print int($3/$2 * 100)}')

if [ "$MEM" -gt 85 ]
then
    echo "Memory usage critical: $MEM%"
else
    echo "Memory usage normal: $MEM%"
fi
```

---

## Expected Output

```text
Memory usage normal: 62%
```

---

# 3. CPU Monitoring Script

## Why Do We Need It?

To identify CPU-intensive processes.

---

## Script

```bash
#!/bin/bash

CPU=$(top -bn1 | grep "Cpu(s)" | awk '{print int($2)}')

if [ "$CPU" -gt 80 ]
then
    echo "CPU usage critical: $CPU%"
else
    echo "CPU usage normal: $CPU%"
fi
```

---

## Common Causes

- Infinite loops
- Java processes
- Database queries
- Runaway scripts

---

# 4. Service Auto-Healing Script

## Why Do We Need It?

Automatically recover failed services.

---

## Script

```bash
#!/bin/bash

SERVICE=nginx

if ! systemctl is-active --quiet $SERVICE
then
    echo "$SERVICE is down."
    systemctl restart $SERVICE

    echo "$SERVICE restarted."
else
    echo "$SERVICE is running."
fi
```

---

## Real-World Use

- Jenkins
- Nginx
- Apache
- MariaDB
- Tomcat

---

## Interview Question

Difference between monitoring and auto-healing?

Monitoring detects issues.

Auto-healing attempts to recover automatically.

---

# 5. Website Health Check Script

## Why Do We Need It?

Verify website availability.

---

## Script

```bash
#!/bin/bash

URL="http://localhost"

STATUS=$(curl -s -o /dev/null -w "%{http_code}" $URL)

if [ "$STATUS" -eq 200 ]
then
    echo "Website Healthy"
else
    echo "Website Down"
fi
```

---

## Expected Output

```text
Website Healthy
```

---

# 6. Database Backup Script

## Why Do We Need It?

Protect business-critical data.

---

## Script

```bash
#!/bin/bash

DB_NAME="companydb"

mysqldump $DB_NAME > /backup/${DB_NAME}.sql

echo "Database backup completed"
```

---

## Production Enhancement

Compress:

```bash
gzip /backup/companydb.sql
```

---

# 7. Backup Transfer Using SCP

## Why Do We Need It?

Store backups on another server.

---

## Script

```bash
#!/bin/bash

FILE=/backup/companydb.sql.gz

scp $FILE backup@backup-server:/backup/
```

---

## Requirements

Passwordless SSH.

```bash
ssh-copy-id backup@backup-server
```

---

## Interview Question

Why should backups be stored remotely?

To protect against:

- Disk failures
- OS corruption
- Server loss

---

# 8. SSL Certificate Expiry Check

## Why Do We Need It?

Prevent website outages from expired certificates.

---

## Script

```bash
#!/bin/bash

openssl x509 -enddate -noout -in certificate.crt
```

---

## Expected Output

```text
notAfter=Dec 20 12:00:00 2026 GMT
```

---

## Real Incident

Many production outages occur because SSL certificates expire unnoticed.

---

# 9. Multi-Service Monitoring Script

## Why Do We Need It?

Quickly check multiple critical services.

---

## Script

```bash
#!/bin/bash

SERVICES=("nginx" "mariadb" "sshd")

for SERVICE in "${SERVICES[@]}"
do
    STATUS=$(systemctl is-active $SERVICE)

    echo "$SERVICE : $STATUS"
done
```

---

## Output

```text
nginx : active
mariadb : active
sshd : active
```

---

# 10. Infrastructure Health Report

## Why Do We Need It?

Perform quick server assessments.

---

## Script

```bash
#!/bin/bash

echo "Hostname:"
hostname

echo ""
echo "Uptime:"
uptime

echo ""
echo "Disk:"
df -h

echo ""
echo "Memory:"
free -h
```

---

## Typical Usage

Daily server health checks.

---

# 11. Port Monitoring Script

## Why Do We Need It?

Verify critical ports are listening.

---

## Script

```bash
#!/bin/bash

PORTS=(22 80 443 3306)

for PORT in "${PORTS[@]}"
do
    ss -tulpn | grep ":$PORT" >/dev/null

    if [ $? -eq 0 ]
    then
        echo "Port $PORT OPEN"
    else
        echo "Port $PORT CLOSED"
    fi
done
```

---

## Output

```text
Port 22 OPEN
Port 80 OPEN
Port 443 OPEN
Port 3306 OPEN
```

---

# 12. Deployment Automation Script

## Why Do We Need It?

Automate application deployment.

---

## Script

```bash
#!/bin/bash

cd /var/www/app

git pull

systemctl restart nginx

echo "Deployment successful"
```

---

## Real-World Workflow

```text
Git Repository
      ↓
Pull Changes
      ↓
Deploy Application
      ↓
Restart Service
```

---

# 13. User Audit Script

## Why Do We Need It?

Identify dormant accounts.

---

## Script

```bash
#!/bin/bash

lastlog | grep "**Never logged in**"
```

---

## Used For

- Security audits
- Compliance
- User cleanup

---

# 14. Log Cleanup Script

## Why Do We Need It?

Prevent log files from consuming disk space.

---

## Script

```bash
#!/bin/bash

find /var/log -type f -mtime +30 -delete

echo "Old logs deleted"
```

---

## Explanation

```bash
-mtime +30
```

Older than 30 days.

---

## Interview Question

Why rotate or delete logs?

To prevent disk exhaustion.

---

# 15. Failed Login Detection Script

## Why Do We Need It?

Identify brute-force attacks.

---

## Script

```bash
#!/bin/bash

grep "Failed password" /var/log/auth.log | tail -20
```

---

## Sample Output

```text
Failed password for invalid user admin
Failed password for root
```

---

## Security Use Cases

- Threat hunting
- Incident response
- Security monitoring

---

# Complete Production Health Check Script

## Why Do We Need It?

This combines all major administrator checks.

---

## Script

```bash
#!/bin/bash

echo "================================="
echo "PRODUCTION HEALTH REPORT"
echo "================================="

echo ""
echo "Date:"
date

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

echo ""
echo "Top CPU Processes:"
ps aux --sort=-%cpu | head

echo ""
echo "Services:"
systemctl is-active nginx
systemctl is-active mariadb
systemctl
