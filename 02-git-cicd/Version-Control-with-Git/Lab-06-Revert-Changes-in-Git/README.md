# Lab 6: Revert Changes in Git

## Objective

Learn how to safely undo the effects of a previous commit using the `git revert` command.

---

## Concept

Mistakes happen during software development. A commit may introduce a bug, break functionality, or add incorrect changes.

Instead of deleting commit history, Git provides the `revert` command to safely undo a specific commit.

The `git revert` command creates a **new commit** that reverses the changes introduced by an earlier commit.

This approach preserves the complete project history and is considered the safest way to undo changes in shared repositories.

---

## Why Use Git Revert?

- Undo a bug-causing commit.
- Reverse unintended changes.
- Maintain complete commit history.
- Avoid rewriting project history.
- Safely work in shared repositories.

---

## How Git Revert Works

Suppose the commit history looks like:

```text
A → B → C → D
```

If commit **C** introduced a bug:

```bash
git revert C
```

Git creates a new commit:

```text
A → B → C → D → E
```

Where:

```text
E = Undo of C
```

The original commit C remains in history, but its changes are reversed.

---

## Step 1: Find the Commit ID

View commit history:

```bash
git log --oneline
```

Example Output:

```text
a1b2c3d Add login page
e4f5g6h Fix navbar issue
abc1234 Add broken validation
```

Identify the commit that needs to be reverted.

---

## Step 2: Revert the Commit

```bash
git revert abc1234
```

Git will:

1. Create a new revert commit.
2. Open an editor for the commit message.
3. Save the inverse changes.

Example commit message:

```text
Revert "Add broken validation"
```

---

## Verify the Result

Check commit history:

```bash
git log --oneline
```

Example:

```text
9z8y7x6 Revert "Add broken validation"
abc1234 Add broken validation
e4f5g6h Fix navbar issue
a1b2c3d Add login page
```

Notice that both commits remain visible.

---

## Revert Without Opening Editor

Automatically create the revert commit:

```bash
git revert --no-edit abc1234
```

Useful for scripting and automation.

---

## Reverting Multiple Commits

Revert several commits one by one:

```bash
git revert commit1
git revert commit2
git revert commit3
```

Or revert a range:

```bash
git revert OLDER_COMMIT..NEWER_COMMIT
```

---

## Git Revert vs Git Reset

### Git Revert

```bash
git revert abc1234
```

- Creates a new commit.
- Preserves history.
- Safe for shared repositories.
- Recommended when commits have already been pushed.

### Git Reset

```bash
git reset --hard abc1234
```

- Rewrites history.
- Removes commits from the current branch.
- Can cause issues for collaborators.
- Use with caution.

---

## Example Scenario

### Commit History

```text
A Add homepage
B Add login page
C Add buggy authentication
D Update styling
```

The authentication changes in commit C cause production issues.

### Solution

Find commit ID:

```bash
git log --oneline
```

Revert the commit:

```bash
git revert C
```

Result:

```text
A
B
C
D
E (Reverts C)
```

The bug is removed while keeping the project history intact.

---

## Typical Workflow

### Check Commit History

```bash
git log --oneline
```

### Identify Problematic Commit

```text
abc1234
```

### Revert the Commit

```bash
git revert abc1234
```

### Verify History

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

- The selected commit's changes are undone.
- A new revert commit is added.
- The original commit remains in history.
- Repository history remains clean and traceable.
- Other team members can safely pull the updated changes.

---

## Real-World DevOps Relevance

Git revert is commonly used by DevOps engineers when:

- A deployment introduces a production issue.
- Infrastructure changes need to be rolled back.
- CI/CD pipeline updates cause failures.
- Kubernetes or Terraform modifications create unexpected behavior.
- A bad commit reaches a shared branch.

Because revert preserves history, it is often preferred over reset in production and team environments.

---

## My Lab Notes

### Commands Executed

```bash
git log --oneline

git revert abc1234
```

### Result

- Located the problematic commit.
- Created a new revert commit.
- Successfully restored the code to its previous state.

### Learning

- Revert does not delete commits.
- Git creates a new commit that undoes previous changes.
- Revert is the safest rollback method for shared repositories.
- Preserving history is important for collaboration and auditing.

---

## Key Takeaways

- Use `git log --oneline` to find commit IDs.
- Use `git revert <commit-id>` to undo a specific commit.
- Revert creates a new commit instead of removing history.
- Revert is safer than reset for shared branches.
- It is the preferred rollback method in team and DevOps environments.
