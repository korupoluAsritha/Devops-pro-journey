# Lab 03: Install and Configure PostgreSQL

## Objective

Learn how to install, configure, and administer PostgreSQL, one of the world's most advanced open-source relational database management systems.

By the end of this lab, you will be able to:

- Understand PostgreSQL architecture
- Install PostgreSQL
- Connect using the `psql` client
- Create databases
- Create roles (users)
- Manage permissions
- Configure remote access
- Troubleshoot PostgreSQL connectivity issues
- Understand PostgreSQL's role in modern applications

---

# What is PostgreSQL?

PostgreSQL (often called **Postgres**) is an advanced open-source relational database management system (RDBMS).

It is designed for:

- Reliability
- Scalability
- Data Integrity
- Performance
- Complex Queries

---

# Why Do We Need PostgreSQL?

Most modern applications require a database.

Example:

```text
User
  ↓
Web Application
  ↓
PostgreSQL
```

The database stores:

- User accounts
- Orders
- Transactions
- Inventory
- Logs
- Application data

---

# Real-World Applications Using PostgreSQL

Examples include:

```text
Banking Applications
ERP Systems
CRM Platforms
Analytics Platforms
Government Systems
Healthcare Applications
```

Many cloud-native applications use PostgreSQL as their primary database.

---

# PostgreSQL vs MySQL/MariaDB

## PostgreSQL

Strengths:

```text
Complex Queries
Advanced Features
Data Integrity
JSON Support
```

---

## MySQL/MariaDB

Strengths:

```text
Simplicity
Web Applications
Ease of Administration
```

---

# PostgreSQL Architecture

```text
Application
      ↓
PostgreSQL Server
      ↓
Database
      ↓
Tables
```

---

# Installing PostgreSQL

Ubuntu/Debian:

```bash
sudo apt update
sudo apt install postgresql -y
```

---

# Verify Installation

Check version:

```bash
psql --version
```

Example:

```text
psql (PostgreSQL) 16.x
```

---

# Verify Service Status

```bash
systemctl status postgresql
```

Expected:

```text
active (running)
```

---

# Understanding System Users

PostgreSQL automatically creates a Linux service account:

```text
postgres
```

Verify:

```bash
id postgres
```

Example:

```text
uid=26(postgres)
```

---

# Understanding PostgreSQL Roles

Unlike MySQL users, PostgreSQL uses:

```text
Roles
```

A role can:

- Login
- Own databases
- Create objects
- Manage permissions

Think of a role as:

```text
User + Permissions
```

---

# Login to PostgreSQL

Switch to postgres account:

```bash
sudo -i -u postgres
```

Launch PostgreSQL shell:

```bash
psql
```

Example:

```text
postgres=#
```

---

# Understanding psql

`psql` is PostgreSQL's interactive command-line tool.

Equivalent to:

```text
mysql
```

for MySQL/MariaDB.

---

# Viewing Databases

Inside psql:

```sql
\l
```

Output:

```text
postgres
template0
template1
```

---

# Creating a Database

```sql
CREATE DATABASE companydb;
```

Verify:

```sql
\l
```

You should see:

```text
companydb
```

---

# Connecting to a Database

```sql
\c companydb
```

Output:

```text
You are now connected to database "companydb"
```

---

# Creating a Table

Example:

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50)
);
```

---

# View Tables

```sql
\dt
```

Output:

```text
employees
```

---

# Insert Records

```sql
INSERT INTO employees(name, department)
VALUES('Asritha','DevOps');
```

---

# Read Records

```sql
SELECT * FROM employees;
```

Output:

```text
 id | name     | department
----+----------+-----------
 1  | Asritha  | DevOps
```

---

# Creating Roles

Create a role:

```sql
CREATE ROLE devops;
```

---

# Create Login Role

```sql
CREATE ROLE devops
LOGIN
PASSWORD 'DevOps@123';
```

---

# Grant Database Access

```sql
GRANT ALL PRIVILEGES ON DATABASE companydb TO devops;
```

---

# Listing Roles

```sql
\du
```

Example:

```text
postgres
devops
```

---

# Exiting PostgreSQL

```sql
\q
```

---

# PostgreSQL File Locations

## Main Configuration

```text
/etc/postgresql/*/main/postgresql.conf
```

---

## Authentication Rules

```text
/etc/postgresql/*/main/pg_hba.conf
```

---

## Logs

```text
/var/log/postgresql/
```

---

# Remote Access Configuration

By default PostgreSQL listens only on:

```text
localhost
```

Meaning:

```text
Only Local Applications Can Connect
```

---

# Verify Listening Address

Check:

```bash
ss -tulpn | grep 5432
```

Example:

```text
127.0.0.1:5432
```

---

# Enable Remote Access

Edit:

```bash
sudo vi /etc/postgresql/*/main/postgresql.conf
```

Find:

```text
listen_addresses = 'localhost'
```

Change:

```text
listen_addresses = '*'
```

---

# Configure Client Access

Edit:

```bash
sudo vi /etc/postgresql/*/main/pg_hba.
