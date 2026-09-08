# Lab 7: Cherry-Pick a Commit in Git

## Objective

Learn how to apply a specific commit from one branch to another using the `git cherry-pick` command.

---

## Concept

In Git, there are situations where you need only one specific change from another branch instead of merging the entire branch.

The `git cherry-pick` command allows you to select a particular commit and apply its changes to your current branch.

Think of it as "copying and pasting" a commit from one branch to another.

---

## Why Use Cherry-Pick?

- Apply a bug fix to another branch.
- Move a specific feature without merging everything.
- Backport fixes from development to production.
- Recover accidentally committed changes.
- Reuse individual commits across branches.

---

## Cherry-Pick vs Merge

### Merge

```bash
git merge feature-branch
```

- Brings all commits from the branch.
- Combines complete branch history.
- Suitable when all changes are needed.

### Cherry-Pick

```bash
git cherry-pick d4e5f6g
```

- Brings only one selected commit.
- Does not merge the entire branch.
- Useful when only a specific change is required.

---

## Example Scenario

Suppose you have two branches:

```text
main

feature-login
```

Commit history:

```text
main:
A --- B

feature-login:
A --- B --- C --- D
```

Where:

```text
C = Fix login bug
D = Add experimental UI
```

You want only the bug fix (C) in the `main` branch.

Instead of merging the entire branch, use cherry-pick.

---

## Step 1: Find the Commit ID

View commit history:

```bash
git log --oneline
```

Example:

```text
d4e5f6g Fix login authentication
a1b2c3d Add login page
```

Copy the commit ID.

---

## Step 2: Switch to Target Branch

Move to the branch that should receive the commit:

```bash
git checkout main
```

or

```bash
git switch main
```

---

## Step 3: Cherry-Pick the Commit

```bash
git cherry-pick d4e5f6g
```

Git applies the changes from that commit and creates a new commit on the current branch.

---

## What Happens Internally?

Before:

```text
main:
A --- B

feature-login:
A --- B --- C
```

After cherry-pick:

```text
main:
A --- B --- C'

feature-login:
A --- B --- C
```

Notice:

```text
C  = Original Commit
C' = New Commit created by cherry-pick
```

The commit is copied, not moved.

---

## Verify the Result

Check commit history:

```bash
git log --oneline
```

You should see a new commit on the current branch containing the selected changes.

---

## Cherry-Pick Multiple Commits

Pick multiple commits individually:

```bash
git cherry-pick commit1
git cherry-pick commit2
```

Or in a single command:

```bash
git cherry-pick commit1 commit2 commit3
```

---

## Cherry-Pick a Range of Commits

Example:

```bash
git cherry-pick commitA..commitD
```

This applies all commits between the specified range.

---

## Handling Cherry-Pick Conflicts

Sometimes the selected commit modifies code that has changed in the target branch.

Git may display:

```text
CONFLICT (content): Merge conflict in app.py
```

Resolve it as follows:

### Edit the File

Remove conflict markers and keep the correct changes.

### Stage the File

```bash
git add app.py
```

### Continue Cherry-Pick

```bash
git cherry-pick --continue
```

---

## Abort Cherry-Pick

If something goes wrong:

```bash
git cherry-pick --abort
```

This restores the repository to its previous state.

---

## Typical Workflow

### View Commit History

```bash
git log --oneline
```

### Switch to Target Branch

```bash
git checkout main
```

### Apply Specific Commit

```bash
git cherry-pick d4e5f6g
```

### Verify Changes

```bash
git log --oneline
```

### Push Changes

```bash
git push origin main
```

---

## Expected Outcome

After completing this lab:

- A specific commit from another branch is applied to the current branch.
- The entire source branch is not merged.
- Git creates a new commit containing the same changes.
- Only the required functionality or fix is transferred.

---

## Real-World DevOps Relevance

Cherry-pick is commonly used in DevOps and software release management.

Examples include:

- Applying a critical production bug fix.
- Backporting security patches to older releases.
- Moving CI/CD pipeline fixes between branches.
- Copying Kubernetes or Terraform updates.
- Delivering urgent fixes without merging unfinished features.

For example:

```text
Development Branch
├── New Features
├── Experimental Changes
└── Critical Bug Fix
```

Instead of merging everything into production, a DevOps engineer may cherry-pick only the critical bug fix commit.

---

## My Lab Notes

### Commands Executed

```bash
git log --oneline

git checkout main

git cherry-pick d4e5f6g
```

### Result

- Located the required commit.
- Switched to the target branch.
- Successfully applied the selected
