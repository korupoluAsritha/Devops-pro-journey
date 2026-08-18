# Real-World Linux Administration Scenarios

## Purpose

This document contains real-world Linux Administration and DevOps scenarios along with the troubleshooting approach and resolution.

The goal is to develop a production mindset:

```text
Problem
   ↓
Investigation
   ↓
Root Cause
   ↓
Fix
   ↓
Verification
```

Never jump directly to a solution.

---

# Scenario 1: MariaDB Service Is Down

## Incident

Developers report:

```text
Application cannot connect to database.
```

---

## Investigation

Check service status:

```bash
systemctl status mariadb
```

Output:

```text
Active: failed
```

Check logs:

```bash
journalctl -u mariadb -n 50
```

Output:

```text
No space left on device
```

---

## Root Cause

Disk is full.

Verify:

```bash
df -h
```

Output:

```text
/ 100% Used
```

---

## Fix

Delete unnecessary files.

Example:

```bash
rm -rf old_logs
```

Restart MariaDB:

```bash
systemctl restart mariadb
```

---

## Verification

```bash
systemctl status mariadb
```

Expected:

```text
Active: active (running)
```

---

# Scenario 2: Website Is Not Accessible

## Incident

Users report:

```text
Website is down.
```

---

## Investigation

Check nginx:

```bash
systemctl status nginx
```

Output:

```text
active (running)
```

Check port:

```bash
ss -tulpn | grep :80
```

No output.

---

## Root Cause

Nginx process running incorrectly or port not listening.

Check logs:

```bash
journalctl -u nginx
```

Output:

```text
bind() failed
Address already in use
```

---

## Fix

Identify process:

```bash
ss -tulpn | grep :80
```

Kill conflicting process:

```bash
kill -15 PID
```

Restart nginx:

```bash
systemctl restart nginx
```

---

## Verification

```bash
ss -tulpn | grep :80
```

Expected:

```text
LISTEN
```

---

# Scenario 3: SSH Login Suddenly Fails

## Incident

A user reports:

```text
SSH login worked yesterday.
Today it doesn't.
```

---

## Investigation

Check account:

```bash
chage -l username
```

Output:

```text
Account expires: Aug 15, 2026
```

Today's date:

```text
Aug 18, 2026
```

---

## Root Cause

Account expired.

---

## Fix

Extend account:

```bash
usermod -e 2026-12-31 username
```

---

## Verification

```bash
chage -l username
```

Should show updated expiry date.

---

# Scenario 4: Ansible Unable To Reach Servers

## Incident

```bash
ansible all -m ping
```

Output:

```text
UNREACHABLE
```

---

## Investigation

Verify manual SSH:

```bash
ssh user@server
```

Output:

```text
Permission denied (publickey)
```

---

## Root Cause

SSH authentication failure.

---

## Fix

Copy key:

```bash
ssh-copy-id user@server
```

---

## Verification

```bash
ssh user@server
```

Should login without password.

Then:

```bash
ansible all -m ping
```

Expected:

```text
pong
```

---

# Scenario 5: Cron Job Did Not Run

## Incident

Nightly backup missing.

---

## Investigation

Verify cron:

```bash
crontab -l
```

Job exists.

Run script manually:

```bash
./backup.sh
```

Output:

```text
command not found
```

---

## Root Cause

Cron environment lacks PATH variables.

---

## Fix

Use full path:

```bash
/usr/bin/mysqldump
```

instead of:

```bash
mysqldump
```

---

## Verification

Check logs:

```bash
cat /var/log/cron_output.log
```

---

# Scenario 6: Bash Script Shows Permission Denied

## Incident

```bash
./monitor.sh
```

Output:

```text
Permission denied
```

---

## Investigation

Check permissions:

```bash
ls -l monitor.sh
```

Output:

```text
-rw-r--r--
```

---

## Root Cause

Execute permission missing.

---

## Fix

```bash
chmod +x monitor.sh
```

---

## Verification

```bash
./monitor.sh
```

Runs successfully.

---

# Scenario 7: Tomcat Is Not Accessible

## Incident

Users cannot access:

```text
http://server:8080
```

---

## Investigation

Check Java:

```bash
java -version
```

Works.

Check Tomcat:

```bash
ps -ef | grep java
```

Nothing.

Check logs:

```bash
tail -f logs/catalina.out
```

Output:

```text
JAVA_HOME not set
```

---

## Root Cause

JAVA_HOME missing.

---

## Fix

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk
```

Start Tomcat.

```bash
./bin/startup.sh
```

---

## Verification

```bash
ss -tulpn | grep :8080
```

Expected:

```text
LISTEN
```

---

# Scenario 8: Server Running Slow

## Incident

Users complain:

```text
Application response is very slow.
```

---

## Investigation

Run:

```bash
top
```

Output:

```text
java 95% CPU
```

---

## Root Cause

Java process consuming excessive CPU.

---

## Fix

Identify application:

```bash
ps -fp PID
```

Gracefully stop:

```bash
kill -15 PID
```

Restart service.

---

## Verification

```bash
top
```

CPU usage returns to normal.

---

# Scenario 9: Firewall Blocking Application

## Incident

Service is running but users cannot access it.

---

## Investigation

Check:

```bash
systemctl status nginx
```

Output:

```text
active (running)
```

Check:

```bash
ss -tulpn | grep :80
```

Output:

```text
LISTEN
```

Check firewall:

```bash
iptables -L -n
```

No rule for port 80.

---

## Root Cause

Firewall blocking traffic.

---

## Fix

```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

---

## Verification

Users can access the website.

---

# Scenario 10: Disk Space Alert

## Incident

Monitoring system sends:

```text
Disk Usage > 95%
```

---

## Investigation

Check:

```bash
df -h
```

Find filesystem.

Check largest directories:

```bash
du -sh /* 2>/dev/null
```

Find large files:

```bash
find / -type f -size +500M
```

---

## Root Cause

Old backups consuming space.

---

## Fix

Archive or remove old backup files.

---

## Verification

```bash
df -h
```

Disk usage reduced.

---

# DevOps Troubleshooting Framework

Whenever something fails:

```text
1. What changed?

2. Is the service running?

3. Is the process running?

4. Is the port listening?

5. Are logs showing errors?

6. Is authentication working?

7. Is storage full?

8. Is the firewall blocking access?

9. Verify fix.

10. Document root cause.
```

---

# Golden Interview Scenario

## Question

A user says:

```text
The application is down.
```

What would you check?

---

## Answer

```text
1. Check service status
2. Check process status
3. Check listening ports
4. Check application logs
5. Check server resources
6. Check firewall rules
7. Check database connectivity
8. Fix root cause
9. Verify service
```

Expected commands:

```bash
systemctl status

ps -ef

ss -tulpn

journalctl

top

df -h

iptables -L -n
```

This demonstrates a troubleshooting methodology 
