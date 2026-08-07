# Lab 02: Temporary User Setup with Expiry

## Objective

Create a temporary Linux user account that automatically expires on a specified date.

This is commonly used for consultants, interns, contractors, auditors, or temporary project members whose access should be automatically revoked after a specific period.

---

# Why Do We Need Temporary Accounts?

In a production environment, not every user needs permanent access.

Examples:

- Interns
- Auditors
- Consultants
- Contractors
- Third-party vendors
- Temporary support engineers

Imagine a consultant joins a project for 3 months.

```text
Consultant Joins
        ↓
Account Created
        ↓
Project Completed
        ↓
Consultant Leaves
        ↓
Account Still Exists
```

If administrators forget to disable the account, it becomes a security risk.

Temporary account expiry helps avoid this situation.

---

# What Is User Account Expiration?

User expiration is a Linux feature that automatically disables a user account after a specified date.

Example:

```text
User: temp_auditor

Expiry Date:
2024-12-31
```

After this date:

```text
Login = Denied
```

The account still exists in Linux but cannot be used to authenticate.

---

# Benefits of Account Expiration

### Improved Security

Old unused accounts cannot be abused.

### Reduced Administrative Effort

No need to manually disable accounts.

### Better Compliance

Many organizations require temporary access to be automatically revoked.

### Principle of Least Privilege

Users only retain access for the duration they actually need.

---

# Creating a Temporary User

Create a user that expires on December 31st, 2024.

```bash
sudo useradd -e 2024-12-31 temp_auditor
```

---

# Command Explanation

```bash
sudo useradd -e 2024-12-31 temp_auditor
```

### sudo

Runs the command with administrative privileges.

---

### useradd

Creates a new Linux user.

---

### -e

Specifies the account expiration date.

---

### 2024-12-31

Expiry date in:

```text
YYYY-MM-DD
```

format.

---

### temp_auditor

The username being created.

---

# Verifying the User Exists

Check the account:

```bash
grep temp_auditor /etc/passwd
```

Example:

```text
temp_auditor:x:1004:1004::/home/temp_auditor:/bin/sh
```

---

# Checking Account Expiration

Use:

```bash
sudo chage -l temp_auditor
```

Example Output:

```text
Last password change                    : Aug 7, 2026
Password expires                        : never
Password inactive                       : never
Account expires                         : Dec 31, 2026
Minimum number of days between password change : 0
Maximum number of days between password change : 99999
```

---

# Understanding chage

The command:

```bash
chage
```

stands for:

```text
Change Age
```

It is used to manage:

- Password expiration
- Password aging
- Account expiration

Useful options:

```bash
chage -l username
```

List aging information.

---

# Modifying an Existing User Expiration

Suppose the user already exists.

Change the expiration date:

```bash
sudo usermod -e 2026-12-31 temp_auditor
```

Verify:

```bash
sudo chage -l temp_auditor
```

---

# Removing Account Expiration

If temporary access becomes permanent:

```bash
sudo usermod -e "" temp_auditor
```

Verify:

```bash
sudo chage -l temp_auditor
```

Expected:

```text
Account expires : never
```

---

# What Happens on Expiry Day?

Suppose:

```text
Username: temp_auditor
Expiry Date: 2026-12-31
```

On:

```text
2027-01-01
```

login attempts fail.

Example:

```bash
ssh temp_auditor@server
```

Output:

```text
Your account has expired.
```

or

```text
Account expired
```

depending on Linux distribution.

---

# Difference Between Account Expiry and Password Expiry

Many beginners confuse these concepts.

## Account Expiry

```text
User Cannot Login
```

The entire account becomes unusable.

---

## Password Expiry

```text
User Must Change Password
```

The account still exists.

The user can regain access by updating the password.

---

# Real World Example

A company hires an external auditor.

Requirements:

```text
Start Date : 2026-09-01
End Date   
