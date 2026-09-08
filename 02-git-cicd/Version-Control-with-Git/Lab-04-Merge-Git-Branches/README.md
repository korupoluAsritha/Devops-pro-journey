
# Lab 4: Merge Git Branches

## Objective

Learn how to merge changes from a feature branch into the main branch using Git.

---

## Concept

Branching allows developers to work on features independently without affecting the main codebase. Once development and testing are complete, the changes need to be integrated back into the main branch.

Git provides the `merge` command to combine changes from one branch into another.

Merging preserves the work done in the feature branch and incorporates it into the target branch.

---

## Why Merge Branches?

- Integrate completed features into the main codebase.
- Combine work from multiple developers.
- Keep the main branch updated with new functionality.
- Support collaborative development workflows.
- Prepare code for testing and deployment.

---

## Scenario

Suppose a developer creates a branch called:

```text
feature-web-login
```

and develops a login feature.

After testing is complete, the feature needs to be merged into the `main` branch.

---

## Merge a Branch

### Step 1: Switch to the Target Branch

The merge is always performed from the branch that will receive the changes.

```bash
git checkout main
```

or

```bash
git switch main
```

---

### Step 2: Merge the Feature Branch

```bash
git merge feature-web-login
```

Git combines all commits from the feature branch into the main branch.

---

## Verify the Merge

Check commit history:

```bash
git log --oneline --graph --all
```

View branches:

```bash
git branch
```

The merged commits should now be part of the `main` branch.

---

## Fast-Forward Merge

When no new commits exist on the target branch, Git performs a fast-forward merge.

Example:

```text
main
  \
   feature-web-login
```

After merge:

```text
main ---- feature-web-login commits
```

Git simply moves the branch pointer forward.

---

## Merge Commit

If both branches have new commits, Git creates a merge commit.

Example:

```text
A---B---C  main
     \
      D---E  feature-web-login
```

After merge:

```text
A---B---C--------M  main
     \          /
      D---E----/
```

Where:

```text
M = Merge Commit
```

This preserves the history of both branches.

---

## Merge Conflicts

Sometimes Git cannot determine which changes should be kept.

Example:

Branch A:

```python
username = "admin"
```

Branch B:

```python
username = "administrator"
```

If both lines were modified independently, Git reports a conflict.

---

## Conflict Example

Git marks the conflicting section:

```text
<<<<<<< HEAD
username = "admin"
=======
username = "administrator"
>>>>>>> feature-web-login
```

---

## Resolve Conflicts

### Step 1: Edit the File

Choose the correct content:

```python
username = "administrator"
```

Remove the conflict markers:

```text
<<<<<<<
=======
>>>>>>>
```

### Step 2: Stage the File

```bash
git add filename
```

### Step 3: Complete the Merge

```bash
git commit
```

Git creates a merge commit after conflicts are resolved.

---

## Typical Development Workflow

### Create Feature Branch

```bash
git switch -c feature-web-login
```

### Make Changes

```bash
vim login.html
```

### Commit Changes

```bash
git add .
git commit -m "Added login page"
```

### Switch to Main

```bash
git checkout main
```

### Merge Feature

```bash
git merge feature-web-login
```

### Push Changes

```bash
git push origin main
```

---

## Expected Outcome

After completing this lab:

- Changes from `feature-web-login` are merged into `main`.
- All commits from the feature branch become part of the target branch.
- The main branch contains the latest functionality.
- The project is ready for further testing or deployment.

---

## Real-World DevOps Relevance

Merging is one of the most common operations in DevOps and CI/CD workflows.

Examples include:

- Merging Infrastructure as Code updates.
- Integrating Kubernetes configuration changes.
- Deploying new application features.
- Combining bug fixes into release branches.
- Completing Pull Request workflows.

In most organizations, developers create feature branches, raise Pull Requests, undergo code reviews, and then merge changes into the main branch before CI/CD pipelines automatically build, test, and deploy the application.

---

## My Lab Notes

### Commands Executed

```bash
git checkout main
git merge feature-web-login
```

### Result

- Successfully switched to the main branch.
- Merged the feature branch.
- Verified changes using Git history.

### Learning

- Merging combines work from different branches.
- Conflicts may occur when the same code is modified in multiple branches.
- Git provides tools to resolve conflicts safely.
- Merging is a core part of team collaboration and CI/CD pipelines.

---

## Key Takeaways

- Use `git merge <branch-name>` to combine branches.
- Always switch to the destination branch before merging.
- Git may perform a fast-forward merge or create a merge commit.
- Conflicts must be resolved manually when Git cannot combine changes automatically.
- Merging is essential for collaborative development and DevOps workflows.
