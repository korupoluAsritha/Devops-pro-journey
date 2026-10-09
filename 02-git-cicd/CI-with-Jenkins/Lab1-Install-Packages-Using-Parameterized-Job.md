# Lab 1: Install Packages Using a Parameterized Jenkins Job

## Objective

Learn how to create a Jenkins job that accepts user input and performs automated package installation on a remote server.

---

## Concept

Jenkins is an automation server widely used in DevOps for Continuous Integration (CI) and Continuous Delivery (CD).

Instead of manually performing repetitive tasks on servers, Jenkins can execute them automatically through jobs.

In this lab, we create a parameterized Jenkins job that installs software packages on a target server based on user input.

For example:

```text
PACKAGE=vim-enhanced
```

When the job runs, Jenkins connects to the target server and installs the specified package automatically.

---

## Why Use Jenkins for Automation?

Manual package installation:

```text
Login to server
↓
Run installation command
↓
Verify installation
```

Automated package installation:

```text
Provide package name
↓
Click Build
↓
Jenkins installs package
```

Benefits:

- Faster execution
- Eliminates repetitive work
- Reduces human errors
- Standardizes operations
- Supports self-service automation

---

## Understanding Parameterized Jobs

A parameterized job accepts input values at runtime.

Example parameter:

```text
PACKAGE
```

Possible values:

```text
vim-enhanced
git
wget
tree
curl
```

Instead of creating separate jobs:

```text
Install-Vim
Install-Git
Install-Wget
```

One parameterized job can handle all package installations.

---

## Jenkins Components Used

### Jenkins Job

A Jenkins job is a task that Jenkins executes.

Example:

```text
install-packages
```

---

### String Parameter

A string parameter allows users to enter text input.

Parameter Name:

```text
PACKAGE
```

Example Value:

```text
vim-enhanced
```

---

### Build

A build is an execution of the Jenkins job.

Example:

```text
Build #1
Build #2
Build #3
```

Each build keeps logs and execution history.

---

## Job Configuration

### Job Name

```text
install-packages
```

### Parameter

```text
PACKAGE
```

### Example Build Value

```text
vim-enhanced
```

---

## Build Workflow

```text
User
  ↓
Enter PACKAGE=vim-enhanced
  ↓
Build Job
  ↓
Jenkins
  ↓
Execute Installation Command
  ↓
Storage Server
  ↓
Package Installed
```

---

## Example Shell Script

A typical Jenkins build step might use:

```bash
sudo yum install -y $PACKAGE
```

or

```bash
sudo dnf install -y $PACKAGE
```

depending on the operating system.

When:

```text
PACKAGE=vim-enhanced
```

Jenkins executes:

```bash
sudo yum install -y vim-enhanced
```

---

## Verifying Installation

After the build succeeds, verify from the storage server.

Example:

```bash
rpm -qa | grep vim-enhanced
```

or

```bash
which vim
```

Expected result:

```text
/usr/bin/vim
```

---

## Typical CI Workflow

Developer/Admin:

```text
Provide input parameter
```

↓

Jenkins:

```text
Triggers automation
```

↓

Target Server:

```text
Installs package
```

↓

Logs:

```text
Success / Failure
```

---

## Expected Outcome

After completing this lab:

- Jenkins is configured successfully.
- A parameterized job is created.
- Package name is provided dynamically.
- Jenkins installs the package automatically.
- Build history is available for auditing and troubleshooting.
- The automation can be reused for multiple packages.

---

## Real-World DevOps Relevance

Parameterized jobs are heavily used in production environments.

Examples:

### Application Deployment

```text
ENVIRONMENT=DEV
ENVIRONMENT=QA
ENVIRONMENT=PROD
```

### Kubernetes Deployment

```text
NAMESPACE=dev
NAMESPACE=staging
NAMESPACE=production
```

### Package Management

```text
PACKAGE=nginx
PACKAGE=docker
PACKAGE=git
```

Instead of creating multiple jobs, engineers use one reusable job with parameters.

---

## CI/CD Concepts Learned

### Continuous Integration (CI)

Continuous Integration automatically builds and validates code changes.

Examples:

- Compile applications
- Run tests
- Package artifacts

---

### Jenkins Job

An automated task executed by Jenkins.

---

### Parameterized Build

A build that accepts user input.

Example:

```text
PACKAGE=vim-enhanced
```

---

### Build History

Jenkins stores:

- Build numbers
- Console output
- Success/failure status
- Execution timestamps

---

## My Lab Notes

### Jenkins Job

```text
install-packages
```

### Parameter

```text
PACKAGE
```

### Test Value

```text
vim-enhanced
```

### Result

```text
Package installed successfully on Storage Server.
```

### Learning

- Jenkins can automate server administration tasks.
- Parameters make jobs reusable.
- Builds provide execution history and logging.
- Automation reduces manual effort and errors.

---

## Interview Questions

### Q1: What is a parameterized build in Jenkins?

**Answer:**

A parameterized build allows users to provide input values at runtime. The same Jenkins job can be reused for different scenarios without changing the job configuration.

---

### Q2: Why use parameters instead of hardcoded values?

**Answer:**

Parameters make jobs flexible, reusable, and easier to maintain.

---

### Q3: What is the benefit of automating package installation through Jenkins?

**Answer:**

It reduces manual effort, ensures consistency, provides auditability, and enables self-service operations.

---

### Key Takeaways

- Jenkins automates repetitive tasks.
- Parameterized jobs accept runtime input.
- One job can manage multiple packages.
- Build logs help in troubleshooting.
- This lab demonstrates a simple but powerful DevOps automation workflow.
