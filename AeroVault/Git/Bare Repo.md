# Using a Windows Bare Repository with an Offline Linux Machine

## Repository Setup

This workflow uses three repositories:

```text
GitLab
  ↕
Windows bare repository
  ↕
Offline Linux working repository
```

The Windows bare repository acts as an intermediary between GitLab and the offline Linux machine.

A bare repository contains Git history and branches, but it does not contain a normal working directory.

---

## Typical Repository Locations

Example Windows bare repository:

```text
C:\Git\project.git
```

Example Linux working repository:

```text
/home/user/project
```

The Linux machine may access the Windows bare repository through SSH or a mounted network share.

---

# Sending Linux Changes to GitLab

## 1. Commit Changes on Linux

From the Linux working repository:

```bash
git status
git add .
git commit -m "Describe the changes"
```

Check the current branch:

```bash
git branch --show-current
```

Assume the branch is named:

```text
feature/my-change
```

## 2. Push from Linux to the Windows Bare Repository

```bash
git push -u origin feature/my-change
```

After the upstream branch has been configured, future pushes can usually use:

```bash
git push
```

This only sends the branch to the Windows bare repository.

It does not automatically send the branch to GitLab.

## 3. Push from the Windows Bare Repository to GitLab

On the Windows machine:

```powershell
git --git-dir="C:\Git\project.git" push origin feature/my-change
```

The branch should now appear on GitLab.

To list the branches stored in the bare repository:

```powershell
git --git-dir="C:\Git\project.git" branch
```

To verify the GitLab remote:

```powershell
git --git-dir="C:\Git\project.git" remote -v
```

The complete upload path is:

```text
Linux working repository
    ↓ git push
Windows bare repository
    ↓ git push
GitLab
```

---

# Sending GitLab Updates to the Offline Linux Machine

## 1. Fetch GitLab Updates into the Windows Bare Repository

On the Windows machine:

```powershell
git --git-dir="C:\Git\project.git" fetch origin
```

This updates remote-tracking references such as:

```text
origin/main
origin/feature/some-branch
```

However, fetching alone may not update the local branch references that the Linux machine sees.

For a specific branch, update the bare repository's local branch with:

```powershell
git --git-dir="C:\Git\project.git" fetch origin main:main
```

For another branch:

```powershell
git --git-dir="C:\Git\project.git" fetch origin feature/example:feature/example
```

This means:

```text
Fetch origin/branch and store it as the local branch named branch.
```

## 2. Fetch Updates on Linux

From the Linux working repository:

```bash
git fetch origin
```

View the available remote branches:

```bash
git branch -r
```

## 3. Update the Current Linux Branch

For an existing local branch:

```bash
git switch main
git pull
```

Alternatively:

```bash
git switch main
git merge origin/main
```

## 4. Check Out a New Branch on Linux

When a new branch exists in the Windows bare repository:

```bash
git fetch origin
git switch --track origin/feature/example
```

An equivalent command is:

```bash
git switch -c feature/example --track origin/feature/example
```

The complete download path is:

```text
GitLab
    ↓ git fetch
Windows bare repository
    ↓ git fetch or git pull
Linux working repository
```

---

# Recommended Daily Workflow

## Linux to GitLab

On Linux:

```bash
git add .
git commit -m "Describe the changes"
git push
```

On Windows:

```powershell
git --git-dir="C:\Git\project.git" push origin <branch-name>
```

## GitLab to Linux

On Windows:

```powershell
git --git-dir="C:\Git\project.git" fetch origin main:main
```

On Linux:

```bash
git switch main
git pull
```

Replace `main` with the desired branch name.

---

# Useful Commands

## Show Windows Bare Repository Branches

```powershell
git --git-dir="C:\Git\project.git" branch
```

## Show All References

```powershell
git --git-dir="C:\Git\project.git" show-ref
```

## Show Configured Remotes

```powershell
git --git-dir="C:\Git\project.git" remote -v
```

## Show Linux Remotes

```bash
git remote -v
```

## Show Linux Local and Remote Branches

```bash
git branch -a
```

## Check Linux Repository Status

```bash
git status
```

---

# Important Notes

- A bare repository does not have files that can be opened and edited normally.
    
- Pushing from Linux to the Windows bare repository does not automatically push to GitLab.
    
- Fetching from GitLab into the Windows bare repository does not automatically update the Linux repository.
    
- Each connection requires a separate Git command.
    
- Avoid using `git push --mirror` unless the Windows bare repository is intended to be an exact mirror of GitLab.
    
- Avoid force-pushing unless you understand which branch history will be overwritten.
    
- Commit or stash Linux changes before pulling updates.
    

The Windows bare repository is effectively a transfer point:

```text
GitLab ↔ Windows bare repository ↔ Offline Linux repository
```