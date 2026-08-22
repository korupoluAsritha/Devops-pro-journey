# Lab 05: Install and Configure a Web Application

## Objective

Learn how to deploy a web application on a Linux web server by placing application files in the web root directory, setting proper ownership and permissions, and verifying that the application is accessible through a browser.

By the end of this lab, you will be able to:

- Understand web application deployment basics
- Deploy application files to a web server
- Configure ownership and permissions
- Troubleshoot deployment issues
- Understand how Apache and Nginx serve applications
- Follow deployment best practices

---

# What is a Web Application Deployment?

Deploying a web application means:

```text
Application Code
        ↓
Copy To Server
        ↓
Configure Permissions
        ↓
Start/Verify Web Service
        ↓
Users Access Application
```

Examples:

- Company Internal Portal
- E-Commerce Website
- HR Application
- Wordpress
- PHP Application
- Static Website

---

# Why Do We Need Deployment?

Developers create application code.

Example:

```text
HTML
CSS
JavaScript
PHP
Images
```

The code must be placed where the web server can access it.

Example:

```text
Browser
   ↓
Apache/Nginx
   ↓
Application Files
```

Without deployment:

```text
Application Exists
But Users Cannot Access It
```

---

# Understanding the Web Root

Most Linux web servers serve content from:

```text
/var/www/html
```

This is called the:

```text
Document Root
```

Anything inside this directory can be served to users.

---

# Apache Default Structure

```text
/var/www/html
│
├── index.html
├── css/
├── images/
└── js/
```

---

# Nginx Default Structure

Usually:

```text
/var/www/html
```

or

```text
/usr/share/nginx/html
```

depending on distribution.

---

# Deployment Workflow

```text
Developer
     ↓
Application Files
     ↓
Copy To Web Root
     ↓
Configure Permissions
     ↓
Verify Website
```

---

# Copy Application Files

Example:

```bash
sudo cp -r ~/my_app/* /var/www/html/
```

---

# Understanding the Command

```bash
cp
```

Copy files.

---

```bash
-r
```

Recursive copy.

Needed for:

```text
Directories
Subdirectories
```

---

```bash
~/my_app/*
```

Source application files.

---

```bash
/var/www/html/
```

Destination directory.

---

# Verify Files

```bash
ls -l /var/www/html
```

Example:

```text
index.php
css/
js/
images/
```

---

# Why Ownership Matters

Files should belong to the web-server process.

For Apache and Nginx on Ubuntu:

```text
www-data
```

owns and serves web content.

---

# Set Ownership

```bash
sudo chown -R www-data:www-data /var/www/html
```

---

# Understanding chown

```bash
chown
```

Changes ownership.

---

```bash
-R
```

Recursive.

Applies ownership
