# Lab 3: Create Git Branches

## Objective

Learn how to create and switch to a new Git branch using the `git switch -c` command.

---

## Concept

A branch in Git is an independent line of development. It allows developers to work on new features, bug fixes, or experiments without affecting the main codebase.

Instead of making all changes directly on the `main` branch, developers create separate branches to keep work isolated and organized.

This makes collaboration easier and reduces the risk of introducing unstable code into the production branch.

---

## Why Use Branches?

- Develop new features safely.
- Fix bugs without impacting stable code.
- Allow multiple developers to work simultaneously.
- Isolate experimental changes.
- Support code reviews through Pull Requests.

---

## Create a New Branch

Git provides the `switch` command to create and move to a new branch.

```bash
git switch -c feature-web-login
```

### What Happens?

- A new branch named `feature-web-login` is created.
- Git automatically switches to that branch.
- Any commits made now belong only to this branch.
- The `main` branch remains unchanged.

---

## Verify Current Branch

To check the branch you are currently working on:

```bash
git branch
```

Example Output:

```text
* feature-web-login
  main
```

The `*` symbol indicates the active branch.

---

## Branch Naming Best Practices

Use meaningful and descriptive names.

### Feature Development

```text
feature-web-login
feature-user-profile
feature-payment-gateway
```

### Bug Fixes

```text
bugfix-header
bugfix-login-error
bugfix-api-timeout
```

### Hotfixes

```text
hotfix-security-patch
hotfix-production-crash
```

### Documentation

```text
docs-readme-update
docs-installation-guide
```

Descriptive names make it easier for team members to understand the purpose of a branch.

---

## View All Branches

List all local branches:

```bash
git branch
```

List both local and remote branches:

```bash
git branch -a
```

---

## Switch Between Branches

Move back to the main branch:

```bash
git switch main
```

Move to an existing branch:

```bash
git switch feature-web-login
```

---

## Typical Development Workflow

### Step 1: Create a Branch

```bash
git switch -c feature-web-login
```

### Step 2: Make Changes

Modify project files.

### Step 3: Commit Changes

```bash
git add .
git commit -m "Add login page"
```

### Step 4: Push Branch

```bash
git push origin feature-web-login
```

### Step 5: Create Pull Request

Submit the branch for review and merge into `main`.

---

## Expected Outcome

After completing this lab:

- A new branch is created.
- Git switches to the new branch automatically.
- Changes made in the branch do not affect the `main` branch.
- Development can continue independently until changes are ready to merge.

---

## Real-World DevOps Relevance

Branching is a fundamental part of modern DevOps workflows.

Common use cases include:

- Developing CI/CD pipeline changes.
- Updating Kubernetes manifests.
- Modifying Infrastructure as Code (Terraform).
- Testing deployment strategies.
- Implementing new application features.

Most organizations follow a branch-based workflow where engineers create feature branches and merge them through Pull Requests after review.

---

## My Lab Notes

### Command Executed

```bash
git switch -c feature-web-login
```

### Result

- Successfully created a new branch.
- Switched from `main` to `feature-web-login`.
- Verified branch creation using `git branch`.

### Learning

- Branches allow isolated development.
- Changes remain separate from the main codebase until merged.
- Descriptive branch names improve collaboration and maintainability.

---

## Key Takeaways

- A branch is an isolated workspace for development.
- `git switch -c <branch-name>` creates and switches to a new branch.
- Branches protect the main codebase from unfinished changes.
- Use descriptive branch names for better team collaboration.
- Branching is a core practice in Git, CI/CD, and DevOps workflows.

