# Lab 1: Clone a Git Repository

## Objective

Learn how to create a local copy of a remote Git repository using the `git clone` command.

---

## Concept

When working on a project, the source code is usually stored in a remote repository such as GitHub, GitLab, or Bitbucket. Before making any changes, developers need to download a complete copy of the repository to their local machine.

Git provides the `git clone` command to accomplish this task.

Cloning a repository creates:

- A complete copy of all project files.
- Full commit history.
- All branches and tags from the remote repository.
- A hidden `.git` directory that tracks version control metadata.

This allows developers to work on the project locally and later synchronize changes with the remote repository.

---

## Why Cloning is Important

- Enables developers to work on the latest source code.
- Preserves complete project history.
- Allows collaboration among multiple team members.
- Provides access to all branches for development and testing.
- Creates a local Git environment connected to the remote repository.

---

## Command Used

```bash
git clone https://github.com/username/repository.git
```

### Example

```bash
git clone https://github.com/korupoluAsritha/Devops-pro-journey.git
```

---

## SSH vs HTTPS

Git repositories can be cloned using either HTTPS or SSH.

### HTTPS

```bash
git clone https://github.com/username/repository.git
```

**Advantages:**

- Easy to configure.
- Works without generating SSH keys.
- Suitable for beginners.

### SSH

```bash
git clone git@github.com:username/repository.git
```

**Advantages:**

- More secure authentication.
- Passwordless access after SSH key setup.
- Preferred for frequent Git operations.

---

## Cloning into a Custom Directory

By default, Git creates a folder with the repository name.

Example:

```bash
git clone https://github.com/username/repository.git
```

Creates:

```text
repository/
```

To use a custom folder name:

```bash
git clone https://github.com/username/repository.git my-project
```

Creates:

```text
my-project/
```

---

## Verification

Navigate into the cloned repository:

```bash
cd Devops-pro-journey
```

Check the remote repository configuration:

```bash
git remote -v
```

Expected output:

```text
origin  https://github.com/korupoluAsritha/Devops-pro-journey.git (fetch)
origin  https://github.com/korupoluAsritha/Devops-pro-journey.git (push)
```

---

## Expected Outcome

After successful execution:

- A new directory is created locally.
- All project files are downloaded.
- A `.git` folder is created.
- The local repository is linked to the remote repository.
- Developers can start making changes and commit updates.

---

## Real-World DevOps Relevance

In DevOps environments, engineers frequently clone repositories to:

- Access infrastructure-as-code repositories.
- Work on CI/CD pipeline configurations.
- Review application source code.
- Troubleshoot issues locally.
- Contribute changes through Git workflows.

Cloning is typically the first Git operation performed when joining a new project or setting up a new workstation.

---

## Key Takeaways

- `git clone` creates a local copy of a remote repository.
- It downloads all files, commits, branches, and tags.
- Both HTTPS and SSH methods are supported.
- A hidden `.git` directory enables Git version tracking.
- Cloning is the starting point for collaborative development workflows.
