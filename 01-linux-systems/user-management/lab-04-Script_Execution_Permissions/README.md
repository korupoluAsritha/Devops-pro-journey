# Lab 04: Script Execution Permissions

## Objective

Understand how Linux controls file execution using permissions and learn how to make a shell script executable.

By the end of this lab, you will be able to:

- Understand Linux file permissions
- Make a script executable using `chmod`
- Verify permissions using `ls -l`
- Understand the purpose of the shebang (`#!/bin/bash`)
- Execute scripts directly from the command line

---

# Why Do We Need Script Execution Permissions?

In Linux, every file is treated as data by default.

Suppose you create a script:

```bash
touch backup_script.sh
```

Linux only sees:

```text
backup_script.sh = File
```

Linux does not automatically assume:

```text
backup_script.sh = Program
```

This prevents accidental execution of unknown files.

Before a file can be executed, Linux requires explicit permission.

This is an important security feature.

---

# Understanding Linux File Types

Linux typically stores:

```text
Regular Files
Directories
Scripts
Executables
Device Files
Links
```

Example:

```bash
ls -l
```

Output:

```text
-rw-r--r--  backup_script.sh
drwxr-xr-x  scripts
```

First character indicates file type.

```text
- = Regular File
d = Directory
l = Symbolic Link
```

---

# Understanding Linux Permissions

Every file has permissions.

Example:

```text
-rwxr-xr-x
```

Let's break it down.

```text
- rwx r-x r-x
  │   │   │
  │   │   └── Others
  │   └────── Group
  └────────── Owner
```

---

# Permission Meaning

## Read (r)

Allows viewing file contents.

Example:

```bash
cat file.txt
```

---

## Write (w)

Allows modifying file contents.

Example:

```bash
vi file.txt
```

---

## Execute (x)

Allows the file to run as a program.

Example:

```bash
./script.sh
```

---

# Permission Categories

Linux permissions are assigned to three groups.

## Owner (u)

The user who owns the file.

Example:

```text
asritha
```

---

## Group (g)

Users belonging to the file's group.

Example:

```text
devops
```

---

## Others (o)

Everyone else on the system.

---

# Example Permission Breakdown

Example:

```text
-rwxr-xr-x
```

Owner:

```text
rwx
```

Can:

- Read
- Write
- Execute

---

Group:

```text
r-x
```

Can:

- Read
- Execute

Cannot:

- Write

---

Others:

```text
r-x
```

Can:

- Read
- Execute

Cannot:

- Write

---

# Creating a Sample Script

Create a script:

```bash
vi backup_script.sh
```

Add:

```bash
#!/bin/bash

echo "Backup Started"
```

Save the file.

---

# What Is a Shebang?

The first line:

```bash
#!/bin/bash
```

is called a:

```text
Shebang
```

or

```text
Hashbang
```

It tells Linux:

> "Use Bash to execute this script."

---

Without Shebang

Linux may not know which interpreter should execute the script.

Possible interpreters:

```text
bash
python
perl
ruby
```

---

Example:

```bash
#!/bin/bash
```

Use Bash.

---

Example:

```python
#!/usr/bin/python3
```

Use Python.

---

# Checking Current Permissions

View permissions:

```bash
ls -l backup_script.sh
```

Output:

```text
-rw-r--r-- 1 user user 29 Aug 7 backup_script.sh
```

Notice:

```text
No x permission
```

The script is not executable.

---

# Trying to Execute Without Permission

Run:

```bash
./backup_script.sh
```

Output:

```text
Permission denied
```

Why?

Because Linux checks:

```text
Execute Bit
```

and finds:

```text
Not Set
```

Execution is blocked.

---

# Adding Execute Permission

Grant execute permission:

```bash
chmod +x backup_script.sh
```

---

# What Does chmod Mean?

`chmod` stands for:

```text
Change Mode
```

It modifies file permissions.

---

Command:

```bash
chmod +x backup_script.sh
```

Meaning:

```text
+  Add Permission
x  Execute Permission
```

---

# Verifying Permissions

Check:

```bash
ls -l backup_script.sh
```

Output:

```text
-rwxr-xr-x 1 user user 29 Aug 7 backup_script.sh
```

Notice:

```text
x
```

is now present.

The file is executable.

---

# Executing the Script

Run:

```bash
./backup_script.sh
```

Output:

```text
Backup Started
```

Success!

---

# Why Use ./ ?

Linux does not automatically search the current directory.

When you type:

```bash
backup_script.sh
```

Linux searches:

```text
PATH Environment Variable
```

Usually:

```text
/usr/bin
/bin
/usr/local/bin
```

Current directory is not searched automatically.

Therefore:

```bash
./backup_script.sh
```

means:

```text
Execute backup_script.sh from current directory
```

---

# Alternative Execution Method

Even without execute permission:

```bash
bash backup_script.sh
```

works because:

```text
Bash reads the file directly
```

Example:

```bash
bash backup_script.sh
```

Output:

```text
Backup Started
```

---

# Direct Execution vs Interpreter Execution

## Direct Execution

```bash
./backup_script.sh
```

Requires:

```text
Execute Permission ✅
Shebang ✅
```

---

## Interpreter Execution

```bash
bash backup_script.sh
```

Requires:

```text
Read Permission ✅
```

Execute permission is not necessary.

---

# Numeric Permission Representation

Linux can represent permissions numerically.

## Values

```text
Read    = 4
Write   = 2
Execute = 1
```

Example:

```text
rwx = 7
rw- = 6
r-x = 5
r-- = 4
```

---

# Common chmod Examples

Grant full permissions to owner:

```bash
chmod 700 backup_script.sh
```

Result:

```text
rwx------
```

---

Owner full access, others read only:

```bash
chmod 744 backup_script.sh
```

Result:

```text
rwxr--r--
```

---

Owner full access, group and others execute:

```bash
chmod 755 backup_script.sh
```

Result:

```text
rwxr-xr-x
```

---

# Security Considerations

Avoid:

```bash
chmod 777 script.sh
```

Result:

```text
rwxrwxrwx
```

Everyone can:

- Read
- Modify
- Execute

This is a security risk.

Prefer:

```bash
chmod 755 script.sh
```

or

```bash
chmod 700 script.sh
```

depending on requirements.

---

# Real DevOps Examples

Common executable files:

```text
deploy.sh
backup.sh
jenkins-agent.sh
setup.sh
install.sh
startup.sh
```

Typical workflow:

```text
Create Script
      ↓
Add Shebang
      ↓
Grant Execute Permission
      ↓
Execute Script
      ↓
Automate Tasks
```

---

# Troubleshooting Script Execution Failures

## Issue 1: Permission Denied

Command:

```bash
./backup_script.sh
```

Error:

```text
Permission denied
```

Fix:

```bash
chmod +x backup_script.sh
```

---

## Issue 2: Bad Interpreter

Error:

```text
bad interpreter: No such file or directory
```

Cause:

```bash
#!/bin/bashh
```

Typo in interpreter path.

Fix:

```bash
#!/bin/bash
```

---

## Issue 3: File Not Found

Error:

```text
./backup_script.sh: No such file or directory
```

Verify:

```bash
ls
```

Ensure script exists.

---

## Issue 4: Wrong Line Endings

Common when scripts are created on Windows.

Error:

```text
/bin/bash^M: bad interpreter
```

Fix:

```bash
dos2unix backup_script.sh
```

---


