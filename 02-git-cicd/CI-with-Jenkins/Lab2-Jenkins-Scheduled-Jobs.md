# Lab 2: Jenkins Scheduled Jobs

## Objective

Learn how to automatically execute Jenkins jobs at predefined times using Jenkins Cron scheduling.

---

## Concept

In a real-world DevOps environment, many tasks need to run automatically without requiring manual intervention.

Examples include:

- Nightly application builds
- Security scans
- Database backups
- Log cleanup tasks
- Health monitoring scripts
- Infrastructure validation checks

Jenkins provides a feature called **Build Periodically**, which allows jobs to run automatically based on a schedule.

This scheduling mechanism uses **Cron syntax**.

---

## Why Scheduled Jobs?

Without scheduling:

```text
Engineer
  ↓
Logs into Jenkins
  ↓
Runs Job
  ↓
Waits for Completion
```

With scheduling:

```text
Scheduled Time
  ↓
Jenkins Triggers Job
  ↓
Job Executes Automatically
  ↓
Results Generated
```

Benefits:

- No manual effort
- Consistent execution
- Reduced operational risks
- Supports 24/7 automation
- Ideal for recurring tasks

---

## Build Periodically

To schedule a Jenkins job:

### Step 1

Open the Jenkins Job.

### Step 2

Select:

```text
Configure
```

### Step 3

Navigate to:

```text
Build Triggers
```

### Step 4

Enable:

```text
Build periodically
```

### Step 5

Enter a Cron schedule.

---

## Understanding Cron Syntax

Jenkins Cron format consists of five fields:

```text
MINUTE HOUR DAY MONTH WEEKDAY
```

Example:

```text
0 0 * * *
```

Meaning:

```text
Minute = 0
Hour = 0
Day = Every Day
Month = Every Month
Weekday = Every Day
```

Result:

```text
Runs daily at midnight
```

---

## Jenkins H Symbol

Jenkins introduces an additional symbol:

```text
H
```

which means:

```text
Hash
```

Jenkins calculates a stable time automatically.

Example:

```text
H 0 * * *
```

Instead of all jobs starting exactly at 00:00, Jenkins distributes load across different minutes.

Benefits:

- Avoids server overload
- Prevents simultaneous job execution
- Improves scalability

---

## Common Scheduling Examples

### Run Every Day at Midnight

```text
H 0 * * *
```

---

### Run Every Hour

```text
H * * * *
```

---

### Run Every 15 Minutes

```text
H/15 * * * *
```

---

### Run Every Day at 2 AM

```text
H 2 * * *
```

---

### Run Every Sunday

```text
H * * * 0
```

---

### Run Monday to Friday at 9 AM

```text
H 9 * * 1-5
```

---

## Real Example

Suppose we have a job:

```text
nightly-security-scan
```

Schedule:

```text
H 0 * * *
```

Workflow:

```text
Midnight
   ↓
Jenkins
   ↓
Runs Security Scan
   ↓
Generates Report
   ↓
Sends Notifications
```

No engineer intervention is required.

---

## Verify Scheduled Builds

After saving the job:

```text
Build History
```

will show automatically triggered builds.

The build cause usually appears as:

```text
Started by timer
```

This confirms Jenkins launched the build according to schedule.

---

## Typical DevOps Use Cases

### CI Builds

```text
Nightly Application Build
```

### Security

```text
Dependency Vulnerability Scan
```

### Infrastructure

```text
Terraform Validation
```

### Operations

```text
Disk Cleanup
```

### Reporting

```text
Generate Daily Reports
```

---

## Expected Outcome

After completing this lab:

- Jenkins automatically triggers builds.
- No manual execution is required.
- The configured schedule executes reliably.
- Build history shows scheduled executions.
- Recurring operational tasks become automated.

---

## Real-World DevOps Relevance

Scheduled jobs are heavily used in production environments.

Examples:

### Infrastructure Maintenance

```text
Cleanup old logs
```

### Monitoring

```text
Daily health checks
```

### Security

```text
Nightly vulnerability scans
```

### CI/CD

```text
Nightly builds and testing
```

Large organizations run thousands of scheduled Jenkins jobs every day to automate routine operations.

---

## Cron Cheat Sheet

```text
*  *  *  *  *
|  |  |  |  |
|  |  |  |  +---- Day of Week (0-7)
|  |  |  +------- Month
|  |  +---------- Day of Month
|  +------------- Hour
+---------------- Minute
```

Example:

```text
30 2 * * *
```

Meaning:

```text
Run every day at 2:30 AM
```

---

## My Lab Notes

### Build Trigger Used

```text
Build periodically
```

### Schedule

```text
H 0 * * *
```

### Result

```text
Jenkins automatically triggered the job.
```

### Learning

- Jenkins can run jobs without manual intervention.
- Cron syntax controls execution schedules.
- The `H` value helps distribute build load.
- Scheduled jobs are essential for automation and infrastructure maintenance.

---

## Interview Questions

### Q1: What is a Jenkins scheduled job?

**Answer:**

A Jenkins scheduled job is a job configured to run automatically at predefined times using the "Build periodically" trigger and Cron expressions.

---
