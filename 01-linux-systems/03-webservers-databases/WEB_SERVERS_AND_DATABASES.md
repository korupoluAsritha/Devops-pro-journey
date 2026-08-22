# MODULE 03: WEB SERVERS AND DATABASES
## Complete Summary Guide for Linux Administration & DevOps

---

# Module Overview

This module focuses on deploying, securing, scaling, and managing web applications and databases in Linux environments.

In real-world production environments, most applications follow a similar architecture:

```text
User
  ↓
DNS
  ↓
Load Balancer
  ↓
Web Server
  ↓
Application Runtime
  ↓
Database
```

This module teaches the core components of that architecture.

---

# Learning Objectives

By completing this module, you learned how to:

✅ Secure websites using SSL/TLS

✅ Configure Nginx as a Load Balancer

✅ Install and manage PostgreSQL

✅ Build a complete LAMP Stack

✅ Deploy web applications

✅ Configure Nginx with PHP-FPM

✅ Set up private Git repositories

✅ Understand modern web infrastructure

---

# Module Roadmap

```text
SSL/TLS Security
       ↓
Nginx Web Server
       ↓
Load Balancing
       ↓
PostgreSQL Database
       ↓
LAMP Stack
       ↓
Application Deployment
       ↓
PHP-FPM Architecture
       ↓
Git Repository Server
       ↓
GitOps Workflow
```

---

# Lab 01: Setup SSL for Nginx

## Why SSL?

SSL/TLS encrypts communication between users and servers.

Without SSL:

```text
Browser
   ↓
Plain Text Traffic
   ↓
Server
```

With SSL:

```text
Browser
   ↓
Encrypted Traffic
   ↓
Server
```

---

## Important Concepts

### HTTP

Port:

```text
80
```

Not encrypted.

---

### HTTPS

Port:

```text
443
```

Encrypted communication.

---

## Nginx SSL Configuration

```nginx
server {
    listen 443 ssl;

    ssl_certificate /etc/ssl/certs/server.crt;

    ssl_certificate_key /etc/ssl/private/server.key;
}
```

---

## Important Commands

Validate Nginx:

```bash
nginx -t
```

Restart:

```bash
systemctl restart nginx
```

Verify:

```bash
ss -tulpn | grep :443
```

---

## Key Learning

Always protect:

```text
server.key
```

Use:

```bash
chmod 600 server.key
```

---

# Lab 02: Nginx Load Balancer

## What Is Load Balancing?

Distributes user requests across multiple servers.

---

## Without Load Balancer

```text
Users
  ↓
Single Server
```

Risk:

```text
Overload
Downtime
```

---

## With Load Balancer

```text
Users
  ↓
Nginx
  ↓
Web01
Web02
Web03
```

---

## Nginx Upstream Configuration

```nginx
upstream my_servers {

    server 10.0.0.1;
    server 10.0.0.2;

}
```

---

## Forward Requests

```nginx
proxy_pass http://my_servers;
```

---

## Load Balancing Algorithms

### Round Robin

Default.

```text
Request1 → Web01
Request2 → Web02
Request3 → Web01
```

---

### Least Connections

```nginx
least_conn;
```

Send traffic to least busy server.

---

### IP Hash

```nginx
ip_hash;
```

Maintains session persistence.

---

## Key Learning

Load balancing improves:

- Availability
- Scalability
- Performance

---

# Lab 03: PostgreSQL Configuration

## What Is PostgreSQL?

Advanced Open Source Relational Database.

Used by:

- Enterprises
- Banks
- Government Systems
- Cloud Applications

---

## Installation

```bash
apt install postgresql -y
```

---

## Access PostgreSQL

```bash
sudo -i -u postgres psql
```

---

## Default Port

```text
5432
```

---

## Create Database

```sql
CREATE DATABASE companydb;
```

---

## Connect Database

```sql
\c companydb
```

---

## Create Table

```sql
CREATE TABLE employees(
 id SERIAL PRIMARY KEY,
 name VARCHAR(100)
);
```

---

## PostgreSQL Roles

PostgreSQL uses:

```text
Roles
```

instead of traditional users.

---

## Important Files

### Configuration

```text
postgresql.conf
```

---

### Authentication

```text
pg_hba.conf
```

---

## Key Learning

Remote connections require:

```text
listen_addresses='*'
```

and proper `pg_hba.conf` rules.

---

# Lab 04: Configure LAMP Server

## What Is LAMP?

```text
L → Linux
A → Apache
M → MySQL
P → PHP
```

---

## Architecture

```text
Browser
   ↓
Apache
   ↓
PHP
   ↓
MySQL
```

---

## Installation

```bash
apt install apache2 mysql-server php \
libapache2-mod-php php-mysql -y
```

---

## Verify Apache

```bash
systemctl status apache2
```

---

## Verify MySQL

```bash
systemctl status mysql
```

---

## Verify PHP

```bash
php -v
```

---

## Test PHP

Create:

```php
<?php
phpinfo();
?>
```

---

## MySQL Hardening

Run:

```bash
mysql_secure_installation
```

---

## Key Learning

LAMP remains the most widely used traditional web hosting stack.

Used by:

- WordPress
- Joomla
- Drupal
- Magento

---

# Lab 05: Web Application Deployment

## Purpose

Deploy an application to a web server.

---

## Deployment Steps

```text
Application Code
       ↓
Copy To Web Root
       ↓
Configure Ownership
       ↓
Set Permissions
       ↓
Serve Application
```

---

## Copy Files

```bash
cp -r ~/my_app/* /var/www/html/
```

---

## Ownership

```bash
chown -R www-data:www-data /var/www/html
```

---

## Permissions

### Directories

```bash
755
```

### Files

```bash
644
```

---

## Remove Default Page

```bash
rm /var/www/html/index.html
```

---

## Key Learning

Most deployment failures occur due to:

- Wrong permissions
- Wrong ownership
- Missing files

---

# Lab 06: Nginx + PHP-FPM Using Unix Socket

## Why PHP-FPM?

Nginx cannot execute PHP directly.

Needs:

```text
Nginx
   ↓
PHP-FPM
   ↓
PHP Code
```

---

## FastCGI Configuration

```nginx
location ~ \.php$ {

    include snippets/fastcgi-php.conf;

    fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
}
```

---

## Why Unix Socket?

### Socket

```text
php8.1-fpm.sock
```

---

### Benefits

✅ Faster

✅ Less CPU

✅ Lower Latency

✅ No Network Overhead

---

## Verify PHP-FPM

```bash
systemctl status php8.1-fpm
```

---

## Common Error

```text
502 Bad Gateway
```

Usually means:

```text
PHP-FPM Down
```

---

## Key Learning

Modern PHP applications typically use:

```text
Nginx
   ↓
PHP-FPM
   ↓
Database
```

---

# Lab 07: Git Repository on Storage Server

## Why?

Create a private Git server.

---

## Architecture

```text
Developer
      ↓
Git Push
      ↓
Storage Server
      ↓
Deployment
```

---

## Create Bare Repository

```bash
git init --bare my_project.git
```

---

## What Is Bare Repository?

Contains:

```text
Commits
Branches
Tags
History
```

But:

```text
No Working Files
```

---

## Add Remote

```bash
git remote add production user@server:/path/repo.git
```

---

## Push Code

```bash
git push production main
```

---

## Git Hooks

Automatic deployment after push.

Example:

```text
git push
      ↓
post-receive
      ↓
Deploy Application
```

---

## Example Hook

```bash
#!/bin/bash

git --work-tree=/var/www/html \
--git-dir=/repos/my_project.git checkout -f
```

---

## Key Learning

This introduces GitOps principles:

```text
Git = Source Of Truth
```

---

# Production Architecture Learned

By the end of Module 3, you understand this complete architecture:

```text
                  Users
                     │
                     ▼
             HTTPS/SSL
                     │
                     ▼
          Nginx Load Balancer
                     │
        ┌────────────┴────────────┐
        ▼                         ▼
    Web Server 1             Web Server 2
        │                         │
        ▼                         ▼
     PHP-FPM                  PHP-FPM
        │                         │
        └────────────┬────────────┘
                     ▼
               PostgreSQL
                 MySQL
                     │
                     ▼
                  Storage
                     │
                     ▼
                Git Server
```

---

# Troubleshooting Framework for Module 3

Whenever a website fails:

```text
1. Check Service
          ↓
2. Check Process
          ↓
3. Check Port
          ↓
4. Check Logs
          ↓
5. Check Permissions
          ↓
6. Check Firewall
          ↓
7. Check Database
          ↓
8. Verify Fix
```

---

## Commands Every DevOps Engineer Should Know

### Service Status

```bash
systemctl status
```

---

### Ports

```bash
ss -tulpn
```

---

### Logs

```bash
journalctl
```

---

### Nginx Validation

```bash
nginx -t
```

---

### Restart Service

```bash
systemctl restart service
```

---

### Database Login

```bash
psql
mysql
```

---

### Git

```bash
git push
git pull
git clone
```

---

# Interview Questions Summary

## Why SSL?

To encrypt communication between clients and servers.

---

## Why Load Balancing?

To distribute traffic and improve availability.

---

## What Is PostgreSQL?

An advanced open-source relational database.

---

## What Is LAMP?

Linux + Apache + MySQL + PHP.

---

## Why Use PHP-FPM?

Nginx cannot execute PHP directly.

---

## Why Use Unix Sockets?

Lower latency and better performance than TCP for local communication.

---

## What Is a Bare Git Repository?

A repository without a working directory used for centralized version control.

---

## What Is GitOps?

Managing deployments and infrastructure through Git as the source of truth.

---

# Key Skills Gained

✅ SSL/TLS Configuration

✅ Nginx Administration

✅ Load Balancing

✅ PostgreSQL Administration

✅ Apache Administration

✅ MySQL Basics

✅ PHP Hosting

✅ PHP-FPM

✅ Application Deployment

✅ Git Server Management

✅ Git Hooks

✅ GitOps Concepts

✅ Web Application Troubleshooting

---

# Module 3 Final Outcome

After completing this module, you can:

```text
Deploy Web Applications
       ↓
Secure Them With SSL
       ↓
Scale Them Using Load Balancers
       ↓
Connect Databases
       ↓
Run PHP Applications
       ↓
Host Applications On Linux
       ↓
Manage Source Code Using Git
       ↓
Automate Deployments Using Git Hooks
```

These skills form the foundation for the next DevOps topics:

```text
Docker
    ↓
Kubernetes
    ↓
CI/CD Pipelines
    ↓
Terraform
    ↓
Cloud Platforms
    ↓
Production DevOps Engineering
```
