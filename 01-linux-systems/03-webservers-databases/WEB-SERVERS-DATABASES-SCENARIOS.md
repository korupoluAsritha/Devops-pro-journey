# Module 03: Real-World Web Servers and Databases Scenarios

## Purpose

This document contains real-world Linux Administration and DevOps scenarios based on Web Servers, Databases, SSL, Load Balancing, PHP-FPM, PostgreSQL, Application Deployment, and GitOps.

The goal is to practice:

```text
Business Requirement
        ↓
Technical Analysis
        ↓
Implementation
        ↓
Verification
        ↓
Troubleshooting
```

This is the exact mindset expected from a Linux Administrator, System Administrator, or Junior DevOps Engineer.

---

# Scenario 01: Secure Load-Balanced Environment for Marketing Campaign

## The Scenario

The marketing team is launching a major campaign tomorrow.

Current architecture:

```text
Users
   ↓
Single Web Server
```

Expected traffic:

```text
3x Normal Traffic
```

The Web Architect wants:

1. Nginx Load Balancer
2. Two Backend Application Servers
3. SSL/HTTPS Security
4. PostgreSQL Database
5. Dedicated PostgreSQL User

---

# Objective

Build:

```text
                 Users
                    │
                    ▼
             HTTPS (443)
                    │
                    ▼
          Nginx Load Balancer
                    │
           ┌────────┴────────┐
           ▼                 ▼
      App Server 1      App Server 2
           │                 │
           └───────┬─────────┘
                   ▼
              PostgreSQL
```

---

# Step 1: Configure PostgreSQL

Install PostgreSQL:

```bash
apt update

apt install postgresql -y
```

Verify:

```bash
systemctl status postgresql
```

Expected:

```text
active (running)
```

---

# Step 2: Create Database

Switch to PostgreSQL:

```bash
sudo -i -u postgres psql
```

Create database:

```sql
CREATE DATABASE campaign_db;
```

---

# Step 3: Create Application User

```sql
CREATE ROLE marketing_app
LOGIN
PASSWORD 'StrongPassword123';
```

Grant privileges:

```sql
GRANT ALL PRIVILEGES
ON DATABASE campaign_db
TO marketing_app;
```

---

# Verification

```sql
\du
```

Verify role exists.

---

# Step 4: Configure Nginx Load Balancer

Create upstream:

```nginx
upstream app_servers {

    server 10.0.0.11;
    server 10.0.0.12;

}
```

---

Create server block:

```nginx
server {

    listen 80;

    location / {
        proxy_pass http://app_servers;
    }
}
```

---

# Validation

```bash
nginx -t
```

---

Reload:

```bash
systemctl reload nginx
```

---

# Step 5: Configure SSL

Add SSL configuration:

```nginx
server {

    listen 443 ssl;

    ssl_certificate     /etc/ssl/certs/server.crt;

    ssl_certificate_key /etc/ssl/private/server.key;

    location / {

        proxy_pass http://app_servers;

    }
}
```

---

# Secure Private Key

```bash
chmod 600 /etc/ssl/private/server.key
```

---

# Validation

```bash
nginx -t
```

---

Restart:

```bash
systemctl restart nginx
```

---

# Verification

Check HTTPS:

```bash
ss -tulpn | grep :443
```

Expected:

```text
LISTEN
```

---

# Success Criteria

✅ HTTPS works

✅ Load balancing distributes traffic

✅ PostgreSQL running

✅ Dedicated database user created

✅ Traffic spread across backend servers

---

# Interview Question

Why put SSL on the load balancer?

Answer:

```text
SSL termination reduces processing load
on backend servers and centralizes
certificate management.
```

---

# Scenario 02: WordPress Website Migration

## Situation

Company wants to migrate WordPress from:

```text
Old Server
```

to

```text
New LAMP Server
```

---

# Task

Deploy WordPress using:

```text
Linux
Apache
MySQL
PHP
```

---

# Solution

Install LAMP:

```bash
apt install apache2 mysql-server php \
libapache2-mod-php php-mysql -y
```

---

Create database:

```sql
CREATE DATABASE wordpress;
```

---

Create user:

```sql
CREATE USER wpuser IDENTIFIED BY 'Password';
```

---

Grant permissions:

```sql
GRANT ALL PRIVILEGES
ON wordpress.*
TO wpuser;
```

---

Deploy files:

```bash
cp -r wordpress/* /var/www/html/
```

---

Fix permissions:

```bash
chown -R www-data:www-data /var/www/html
```

---

Verify:

```text
http://server-ip
```

---

# Scenario 03: PHP Website Showing 502 Bad Gateway

## Situation

Users report:

```text
Website returns:

502 Bad Gateway
```

---

# Investigation

Check nginx:

```bash
systemctl status nginx
```

Running.

---

Check PHP-FPM:

```bash
systemctl status php8.1-fpm
```

Output:

```text
inactive
```

---

# Root Cause

PHP-FPM stopped.

---

# Fix

```bash
systemctl restart php8.1-fpm
```

---

# Verify

```bash
systemctl status php8.1-fpm
```

Expected:

```text
active (running)
```

---

# Scenario 04: Users Report SSL Warnings

## Situation

Website opens.

Browser shows:

```text
Not Secure
```

---

# Investigation

Check certificate:

```bash
openssl x509 \
-enddate \
-noout \
-in server.crt
```

Output:

```text
Certificate Expired
```

---

# Root Cause

Expired SSL certificate.

---

# Fix

Replace certificate.

Reload:

```bash
systemctl reload nginx
```

---

# Verification

```text
Padlock Visible
```

---

# Scenario 05: PostgreSQL Remote Access Failure

## Situation

Developers cannot connect remotely.

---

# Investigation

Verify:

```bash
ss -tulpn | grep 5432
```

Output:

```text
127.0.0.1:5432
```

---

# Root Cause

PostgreSQL listening only locally.

---

# Fix

Edit:

```text
postgresql.conf
```

Change:

```text
listen_addresses='*'
```

---

Edit:

```text
pg_hba.conf
```

Add:

```text
host all all 0.0.0.0/0 md5
```

---

Restart:

```bash
systemctl restart postgresql
```

---

# Verification

```bash
ss -tulpn | grep 5432
```

Expected:

```text
0.0.0.0:5432
```

---

# Scenario 06: Nginx Load Balancer Not Distributing Traffic

## Situation

All traffic reaches:

```text
Web01
```

None reaches:

```text
Web02
```

---

# Investigation

Check upstream.

Found:

```nginx
upstream app_servers {
    server 10.0.0.11;
}
```

---

# Root Cause

Second backend missing.

---

# Fix

```nginx
upstream app_servers {

    server 10.0.0.11;

    server 10.0.0.12;

}
```

---

Reload:

```bash
systemctl reload nginx
```

---

# Verification

Refresh repeatedly.

Observe:

```text
Request → Web01
Request → Web02
Request → Web01
```

---

# Scenario 07: Deployment Not Visible After Git Push

## Situation

Developer executes:

```bash
git push production main
```

Push succeeds.

Website unchanged.

---

# Investigation

Check hook:

```bash
ls -l hooks/post-receive
```

Output:

```text
-rw-r--r--
```

---

# Root Cause

Hook not executable.

---

# Fix

```bash
chmod +x hooks/post-receive
```

---

# Verification

Push code again.

Application updates automatically.

---

# Scenario 08: New Web Application Returns 403 Forbidden

## Situation

Application deployed.

Browser:

```text
403 Forbidden
```

---

# Investigation

Check ownership:

```bash
ls -l /var/www/html
```

Output:

```text
root root
```

---

# Root Cause

Incorrect ownership.

---

# Fix

```bash
chown -R www-data:www-data /var/www/html
```

---

Apply permissions:

```bash
find /var/www/html \
-type d \
-exec chmod 755 {} \;

find /var/www/html \
-type f \
-exec chmod 644 {} \;
```

---

# Verification

Application loads successfully.

---

# Scenario 09: Campaign Traffic Causes Slow Response

## Situation

Traffic increased 4x.

Users experience delays.

---

# Investigation

Check load balancer.

Only 2 backend servers.

CPU:

```bash
top
```

Shows:

```text
95% CPU
```

---

# Solution

Add additional backend:

```nginx
upstream app_servers {

    server 10.0.0.11;
    server 10.0.0.12;
    server 10.0.0.13;

}
```

---

Reload:

```bash
systemctl reload nginx
```

---

# Result

Traffic distributed.

Performance improves.

---

# Scenario 10: Complete Production Troubleshooting

## Incident

Application unavailable.

---

# Troubleshooting Process

### Step 1

Check Nginx:

```bash
systemctl status nginx
```

---

### Step 2

Check Ports:

```bash
ss -tulpn
```

---

### Step 3

Check SSL:

```bash
openssl x509 \
-enddate \
-noout \
-in server.crt
```

---

### Step 4

Check PHP-FPM:

```bash
systemctl status php8.1-fpm
```

---

### Step 5

Check PostgreSQL:

```bash
systemctl status postgresql
```

---

### Step 6

Check Logs:

```bash
journalctl -xe
```

---

### Step 7

Verify Backend Servers:

```bash
curl http://10.0.0.11
curl http://10.0.0.12
```

---

### Step 8

Implement Fix

---

### Step 9

Verify Service

---

# Golden Interview Scenario

## Question

Tomorrow your company expects 5x traffic due to a large marketing campaign. How would you prepare the infrastructure?

---

## Answer

```text
1. Configure SSL for security

2. Deploy Nginx Load Balancer

3. Add multiple backend servers

4. Use PHP-FPM for performance

5. Configure PostgreSQL database

6. Create dedicated database users

7. Verify backups

8. Test failover

9. Validate HTTPS

10. Perform load testing
```

---

# Module 03 Troubleshooting Framework

```text
Website Down
      ↓
Check Nginx
      ↓
Check SSL
      ↓
Check Port 80/443
      ↓
Check Backend Servers
      ↓
Check PHP-FPM
      ↓
Check Database
      ↓
Check Permissions
      ↓
Check Logs
      ↓
Fix Issue
      ↓
Verify
```

---

# Key DevOps Skills Practiced

✅ SSL/TLS

✅ Nginx Load Balancing

✅ PostgreSQL Administration

✅ PHP-FPM

✅ Application Deployment

✅ File Permissions

✅ GitOps

✅ Git Hooks

✅ Troubleshooting

✅ Production Architecture Design

These scenarios closely mirror what Junior Linux Administrators, System Administrators, and DevOps Engineers encounter in real production environments and technical interviews.
