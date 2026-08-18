# System Administration Interview Questions and Answers

# Ansible

## What is Ansible?

Ansible is an open-source automation and configuration management tool used to manage infrastructure, deploy applications, and automate operational tasks.

---

## Why is Ansible called Agentless?

Because it communicates through SSH and does not require a dedicated agent on managed nodes.

---

## What is an Inventory File?

A file that contains the list of servers managed by Ansible.

Default:

```bash
/etc/ansible/hosts
```

---

## How do you verify Ansible connectivity?

```bash
ansible all -m ping
```

---

## What does "pong" mean?

It confirms successful communication between the control node and managed node.

---

# MariaDB Troubleshooting

## What is the first command you run when MariaDB is down?

```bash
systemctl status mariadb
```

---

## Where would you look for MariaDB errors?

```bash
journalctl -u mariadb
```

and

```bash
/var/log/mysql/error.log
```

---

## How do you check if the database port is active?

```bash
ss -tulpn | grep 3306
```

---

## Common MariaDB startup failures?

- Disk full
- Permission issues
- Port conflict
- Invalid configuration
- Corrupted database files

---

# Bash Scripting

## What is Bash?

The Bourne Again Shell used for interacting with Linux systems.

---

## What is a Shebang?

The first line of a script that specifies the interpreter.

Example:

```bash
#!/bin/bash
```

---

## How do you declare a variable?

```bash
NAME=value
```

---

## How do you debug a Bash script?

```bash
bash -x script.sh
```

---

## What is command substitution?

```bash
DATE=$(date)
```

Stores command output in a variable.

---

# Apache Tomcat

## What is Tomcat?

A Java web server and servlet container used to host Java applications.

---

## Which command starts Tomcat?

```bash
./bin/startup.sh
```

---

## Which command stops Tomcat?

```bash
./bin/shutdown.sh
```

---

## What port does Tomcat use by default?

```text
8080
```

---

## Most important Tomcat log file?

```text
logs/catalina.out
```

---

## Why would Tomcat fail to start?

- Java missing
- JAVA_HOME not configured
- Port conflict
- Configuration issue

---

# Network Services

## What does ss do?

Displays socket and network connection information.

---

## Explain ss -tulpn.

```text
-t TCP
-u UDP
-l Listening
-p Process
-n Numeric
```

---

## How do you verify Nginx is listening?

```bash
ss -tulpn | grep :80
```

---

## How do you identify who owns a port?

```bash
ss -tulpn
```

---

# IPTables

## What is IPTables?

Linux packet filtering firewall.

---

## What does the INPUT chain do?

Controls incoming traffic.

---

## Allow HTTP traffic.

```bash
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

---

## View firewall rules.

```bash
iptables -L -n
```

---

## Why is rule order important?

IPTables processes rules from top to bottom and stops at the first matching rule.

---

# Process Troubleshooting

## What is a process?

A running instance of a program.

---

## What is a PID?

A Process ID uniquely identifying a running process.

---

## How do you monitor active processes?

```bash
top
```

or

```bash
htop
```

---

## Difference between SIGTERM and SIGKILL?

SIGTERM:

```bash
kill -15 PID
```

Graceful shutdown.

SIGKILL:

```bash
kill -9 PID
```

Forced shutdown.

---

## Why should SIGTERM be attempted first?

It allows applications to:

- Save data
- Close files
- Release resources

before exiting.

---

# Scenario-Based Questions

## Application is Down. What Do You Check?

1. Service status

```bash
systemctl status service
```

2. Process

```bash
ps -ef
```

3. Port

```bash
ss -tulpn
```

4. Logs

```bash
journalctl
```

5. Firewall

```bash
iptables -L -n
```

---

## Website is Running But Not Reachable.

Check:

1. Service status
2. Port listening
3. Firewall
4. Logs
5. DNS (if applicable)

---

## Server is Slow. What Would You Do?

Check:

```bash
top
```

Identify:

- CPU usage
- Memory usage
- Resource-heavy processes

Investigate and terminate if necessary.

---

## Ansible Returns UNREACHABLE.

Check:

1. SSH connectivity

```bash
ssh user@server
```

2. Inventory file

3. Network access

4. SSH keys

---

## Cron Job Didn't Run.

Check:

1. Cron service

```bash
systemctl status crond
```

2. Crontab

```bash
crontab -l
```

3. Script execution

4. Logs

5. Absolute paths

---

## Production Mindset Question

What is the most important troubleshooting principle?

Answer:

```text
Do not make assumptions.

Verify.
Collect evidence.
Identify root cause.
Implement fix.
Validate solution.
Document findings.
```

This mindset is what separates a beginner from a professional Linux Administrator or DevOps Engineer.
