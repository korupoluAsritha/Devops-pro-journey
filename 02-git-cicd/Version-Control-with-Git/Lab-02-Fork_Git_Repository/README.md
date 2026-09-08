# Lab 2: Fork a Git Repository

## Objective

Learn how to create a personal copy of another user's repository using the **Fork** feature and prepare it for contributing changes.

---

## Concept

Forking is a Git hosting platform feature (GitHub, GitLab, Bitbucket) that creates a copy of someone else's repository under your own account.

Unlike cloning, which creates a local copy on your machine, forking creates a copy on the remote platform. This allows you to freely experiment, develop features, and submit contributions without affecting the original project.

Forking is commonly used in Open Source development where contributors do not have direct write access to the original repository.

---

## Why Fork a Repository?

- Contribute to Open Source projects.
- Experiment with changes safely.
- Develop new features independently.
- Fix bugs and submit improvements.
- Maintain your own version of a project.

---

## Fork vs Clone

| Fork | Clone |
|--------|--------|
| Creates a copy on GitHub/GitLab/Bitbucket | Creates a copy on your local machine |
| Used when you don't have write access | Used to download code locally |
| Lives under your account | Lives on your workstation |
| Can be synchronized with original repository | Connected to a remote repository |

---

## Steps to Fork a Repository

### Step 1: Open the Repository

Navigate to the repository you want to contribute to.

Example:

```text
https://github.com/original-owner/project-name
```

### Step 2: Click the Fork Button

On GitHub, click the **Fork** button located in the top-right corner.

GitHub creates a copy under your account:

```text
https://github.com/YOUR_USERNAME/project-name
```

---

## Clone Your Fork

Once the fork is created, clone it to your local machine.

```bash
git clone https://github.com/YOUR_USERNAME/forked-repo.git
```

### Example

```bash
git clone https://github.com/korupoluAsritha/project-name.git
```

Move into the repository:

```bash
cd project-name
```

---

## Configure the Upstream Repository

After cloning your fork, add the original repository as an **upstream** remote.

This allows you to receive future updates from the original repository.

```bash
git remote add upstream https://github.com/original-owner/project-name.git
```

Verify the configuration:

```bash
git remote -v
```

Expected output:

```text
origin    https://github.com/YOUR_USERNAME/project-name.git (fetch)
origin    https://github.com/YOUR_USERNAME/project-name.git (push)

upstream  https://github.com/original-owner/project-name.git (fetch)
upstream  https://github.com/original-owner/project-name.git (push)
```

---

## Why Upstream is Important

The original repository continues to evolve as maintainers merge changes.

To keep your fork updated:

```bash
git fetch upstream
git merge upstream/main
```

or

```bash
git pull upstream main
```

Benefits:

- Keeps your fork in sync with the latest changes.
- Reduces merge conflicts.
- Ensures contributions are built on the latest codebase.

---

## Typical Open Source Workflow

### 1. Fork Repository

Create a copy under your GitHub account.

### 2. Clone Fork

```bash
git clone https://github.com/YOUR_USERNAME/project.git
```

### 3. Create a Feature Branch

```bash
git checkout -b feature-fix
```

### 4. Make Changes

Modify files as required.

### 5. Commit Changes

```bash
git add .
git commit -m "Fixed issue"
```

### 6. Push to Fork

```bash
git push origin feature-fix
```

### 7. Create Pull Request

Submit a Pull Request from your fork to the original repository.

Project maintainers review and merge your changes.

---

## Expected Outcome

After completing this lab:

- A forked repository exists under your GitHub account.
- The fork is cloned locally.
- The original repository is configured as an upstream remote.
- You have full push access to your fork.
- You can safely develop and submit contributions through Pull Requests.

---

## Real-World DevOps Relevance

Forking is frequently used by DevOps engineers when:

- Contributing to Open Source projects.
- Improving Terraform modules.
- Enhancing Kubernetes manifests.
- Fixing CI/CD pipeline configurations.
- Collaborating on shared infrastructure repositories.

Many popular projects such as Kubernetes, Helm, Docker, Terraform, and Jenkins rely on the fork-and-pull-request workflow for community contributions.

---

## Key Takeaways

- Forking creates a personal copy of another repository on GitHub.
- It is used when direct write access is unavailable.
- Clone the fork to work locally.
- Add the original repository as an upstream remote.
- Keep your fork synchronized with upstream changes.
- Fork + Pull Request is the standard Open Source contribution model.
