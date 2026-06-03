### From Zero to Published — Everything You Need to Master GitHub

---

## Table of Contents

1. [What You Need Before Starting](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#1-what-you-need-before-starting)
2. [Setting Up Git on Your Computer](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#2-setting-up-git-on-your-computer)
3. [Creating Your GitHub Account & Repository](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#3-creating-your-github-account--repository)
4. [Publishing Your First Project](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#4-publishing-your-first-project)
5. [Understanding the GitHub Workflow](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#5-understanding-the-github-workflow)
6. [Editing & Updating Your Project](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#6-editing--updating-your-project)
7. [Syncing Between Devices](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#7-syncing-between-devices)
8. [Branches — Working Safely](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#8-branches--working-safely)
9. [Writing a Great README](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#9-writing-a-great-readme)
10. [Private vs Public Repositories](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#10-private-vs-public-repositories)
11. [Collaborating with Others](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#11-collaborating-with-others)
12. [Common Problems & Fixes](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#12-common-problems--fixes)
13. [Essential Git Commands Cheat Sheet](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#13-essential-git-commands-cheat-sheet)
14. [Best Practices & Pro Tips](https://claude.ai/chat/cc08b116-59c4-4ee2-91f4-c3b2d5d604f1#14-best-practices--pro-tips)

---

## 1. What You Need Before Starting

|Requirement|Details|
|---|---|
|**Git**|Free version control software — [git-scm.com](https://git-scm.com/)|
|**GitHub Account**|Free at [github.com](https://github.com/)|
|**A terminal / command prompt**|Built into every OS (Terminal on Mac/Linux, CMD or PowerShell on Windows)|
|**Your project files**|Any folder with code, documents, or assets|

> **What's the difference between Git and GitHub?**
> 
> - **Git** is the tool installed on your computer that tracks changes to files.
> - **GitHub** is the website that stores your project online so it's backed up and shareable.

---

## 2. Setting Up Git on Your Computer

### Install Git

- **Windows:** Download from [git-scm.com](https://git-scm.com/) and run the installer.
- **Mac:** Run `git --version` in Terminal — if not installed, it will prompt you automatically.
- **Linux:** Run `sudo apt install git`

### Configure Your Identity (One-Time Setup)

Open your terminal and run these two commands. Git will attach your name and email to every change you make.

```bash
git config --global user.name "Your Full Name"
git config --global user.email "your@email.com"
```

Confirm everything is saved:

```bash
git config --list
```

### Set Up SSH Authentication (Recommended — do this once)

SSH lets you push to GitHub without typing your password every time.

```bash
# Step 1: Generate an SSH key
ssh-keygen -t ed25519 -C "your@email.com"
# Press Enter 3 times to accept defaults

# Step 2: Copy the key to your clipboard
# Mac:
cat ~/.ssh/id_ed25519.pub | pbcopy

# Windows (PowerShell):
Get-Content ~/.ssh/id_ed25519.pub | Set-Clipboard

# Linux:
cat ~/.ssh/id_ed25519.pub
# Then manually copy the output
```

**Step 3:** Go to GitHub → Your Avatar (top right) → **Settings** → **SSH and GPG keys** → **New SSH key** → Paste your key → Save.

**Step 4:** Test it:

```bash
ssh -T git@github.com
# You should see: "Hi username! You've successfully authenticated."
```

---

## 3. Creating Your GitHub Account & Repository

### Create Your Account

1. Go to [github.com](https://github.com/) and sign up.
2. Verify your email address.
3. Choose the **Free** plan (it includes unlimited public AND private repositories).

### Create a New Repository

A **repository (repo)** is the container that holds your project on GitHub.

1. Click the **+** icon (top right) → **New repository**
2. Fill in the form:

|Field|What to Enter|
|---|---|
|**Repository name**|Short, descriptive, no spaces (use hyphens: `my-cool-project`)|
|**Description**|One sentence about what the project does (optional but helpful)|
|**Visibility**|Public or Private (see Section 10)|
|**Initialize with README**|✅ Check this ONLY if you don't have existing files to upload|
|**Add .gitignore**|Choose your language (e.g., Python, Node) to auto-exclude junk files|
|**License**|MIT is a safe, permissive default for open-source projects|

3. Click **Create repository**.

---

## 4. Publishing Your First Project

You have two scenarios — choose the one that matches you.

---

### Scenario A — You have existing project files on your computer

```bash
# Step 1: Open your terminal and navigate into your project folder
cd /path/to/your/project
# Example: cd ~/Documents/my-cool-project

# Step 2: Initialize Git in this folder (only do this once per project)
git init

# Step 3: Connect it to your GitHub repository
# Replace YOUR_USERNAME and YOUR_REPO_NAME with your actual values
git remote add origin git@github.com:YOUR_USERNAME/YOUR_REPO_NAME.git

# Step 4: Stage all your files (prepare them for upload)
git add .

# Step 5: Create your first commit (a snapshot with a message)
git commit -m "Initial commit — first upload"

# Step 6: Push (upload) to GitHub
git push -u origin main
```

> **What does `-u origin main` mean?** `-u` sets the default so next time you can just type `git push` instead of the full command. `origin` is the nickname for your GitHub remote. `main` is the branch name.

---

### Scenario B — You created the repo on GitHub first (with a README)

```bash
# Step 1: Clone (download) the repository to your computer
git clone git@github.com:YOUR_USERNAME/YOUR_REPO_NAME.git

# Step 2: Move into the downloaded folder
cd YOUR_REPO_NAME

# Step 3: Copy your project files into this folder
# (Do this manually in Finder / File Explorer, or via terminal)

# Step 4: Stage, commit, and push
git add .
git commit -m "Add project files"
git push
```

---

## 5. Understanding the GitHub Workflow

Every time you make changes, you follow this 3-step cycle:

```
[ Edit files ] → [ git add ] → [ git commit ] → [ git push ]
```

### The Three Zones

```
Your Files          Staging Area         Local Repo         GitHub (Remote)
(Working Dir)       (Index)              (.git folder)      (github.com)

   Edit  ──git add──►  Staged  ──git commit──►  Saved  ──git push──►  Online
         ◄──git restore──       ◄──git reset──          ◄──git pull──
```

|Command|What it Does|
|---|---|
|`git add .`|Stages ALL changed files|
|`git add filename.py`|Stages only one specific file|
|`git commit -m "message"`|Saves a snapshot of staged files|
|`git push`|Uploads commits to GitHub|
|`git pull`|Downloads latest changes from GitHub|
|`git status`|Shows what's changed and what's staged|
|`git log`|Shows history of all commits|

---

## 6. Editing & Updating Your Project

After your project is published, every future update follows this pattern:

```bash
# 1. Make your changes to files using any editor (VS Code, Notepad, etc.)

# 2. Check what changed
git status

# 3. Stage the changes
git add .

# 4. Commit with a meaningful message
git commit -m "Fix login bug on mobile"

# 5. Push to GitHub
git push
```

### Writing Good Commit Messages

A good message explains **what** changed and **why**, not how.

```
✅ Good:  "Fix crash when user uploads empty file"
✅ Good:  "Add dark mode toggle to settings page"
✅ Good:  "Update README with installation steps"

❌ Bad:   "fix"
❌ Bad:   "changes"
❌ Bad:   "asdfgh"
```

### Undoing Changes

```bash
# Undo unsaved changes in a file (reverts to last commit)
git restore filename.py

# Unstage a file (remove from staging but keep edits)
git restore --staged filename.py

# Undo the last commit but KEEP the file changes
git reset --soft HEAD~1

# View what changed in each commit
git log --oneline
```

---

## 7. Syncing Between Devices

If you work on multiple computers (e.g., home and office), always sync before starting work.

### On Computer A (where you made changes):

```bash
git add .
git commit -m "Your message"
git push
```

### On Computer B (before you start working):

```bash
git pull
```

> **Rule of thumb:** Always `git pull` before you start working, and always `git push` when you're done. This prevents conflicts.

### If You Get a Merge Conflict

A conflict happens when two versions of the same line differ. Git will mark the file like this:

```
<<<<<<< HEAD
Your local version of the line
=======
The version from GitHub
>>>>>>> origin/main
```

**To fix it:**

1. Open the file in your editor.
2. Decide which version to keep (or combine them).
3. Delete the `<<<<<<<`, `=======`, and `>>>>>>>` markers.
4. Save, then run `git add .` → `git commit -m "Resolve merge conflict"` → `git push`.

---

## 8. Branches — Working Safely

Branches let you experiment without breaking your main project.

```
main ────────●────────────────────────────────────●── (stable)
              \                                  /
feature/login  ●────●────●────●────●────●──────●  (your experiments)
```

### Common Branch Workflow

```bash
# Create and switch to a new branch
git checkout -b feature/new-login-page

# Work on your files, then add and commit normally
git add .
git commit -m "Build new login UI"

# Push the branch to GitHub
git push -u origin feature/new-login-page

# When done, merge it into main
git checkout main
git merge feature/new-login-page

# Push the updated main to GitHub
git push

# Delete the old branch (optional cleanup)
git branch -d feature/new-login-page
```

> On GitHub, you can also merge branches using a **Pull Request (PR)** — a way to review changes before merging. Great for teams.

---

## 9. Writing a Great README

Your `README.md` is the first thing visitors see. Use this template:

````markdown
# Project Name

A one-paragraph description of what this project does and who it's for.

## Features
- Feature 1
- Feature 2
- Feature 3

## Installation

```bash
git clone https://github.com/yourusername/your-repo.git
cd your-repo
npm install     # or pip install -r requirements.txt, etc.
````

## Usage

```bash
npm start
```

## Screenshots

![App screenshot](https://claude.ai/chat/screenshot.png)

## Technologies Used

- React
- Python 3.11
- PostgreSQL

## License

MIT — see [LICENSE](https://claude.ai/chat/LICENSE) for details.

````

---

## 10. Private vs Public Repositories

| Feature | Public | Private |
|---|---|---|
| Anyone can see it | ✅ Yes | ❌ No |
| You control who can edit | ✅ Yes | ✅ Yes |
| Shows on your profile | ✅ Yes | ❌ No |
| Great for portfolios | ✅ Yes | — |
| Free on GitHub | ✅ Yes | ✅ Yes |
| Can be made open-source | ✅ Yes | ❌ No |

### How to Switch Visibility
1. Go to your repo on GitHub.
2. Click **Settings** → scroll to **Danger Zone**.
3. Click **Change repository visibility**.

### What to Keep Private
- Projects with passwords, API keys, or secrets in the code
- Client work or proprietary code
- Unfinished work you're not ready to share

> **⚠️ Important:** Never commit passwords or API keys to Git — even in private repos. Use a `.env` file and add it to `.gitignore`.

---

## 11. Collaborating with Others

### Adding a Collaborator (Private Repos)
1. Go to your repo → **Settings** → **Collaborators** → **Add people**.
2. Search by their GitHub username or email.

### Forking (Contributing to Someone Else's Project)
1. Click **Fork** on their repo — this copies it to your account.
2. Clone your fork, make changes, push to your fork.
3. Click **New Pull Request** to suggest your changes to them.

### Cloning Someone Else's Public Repo
```bash
git clone https://github.com/theirusername/their-repo.git
````

Note: You can clone it but you can't push back to it unless they give you access.

---

## 12. Common Problems & Fixes

### "Permission denied (publickey)"

Your SSH key isn't set up. Go back to Section 2 and complete the SSH setup.

### "Failed to push — updates were rejected"

Someone else (or another device) pushed changes you don't have yet.

```bash
git pull
# Resolve any conflicts, then:
git push
```

### "fatal: not a git repository"

You're not inside a Git folder. Use `cd` to navigate into your project folder, or run `git init`.

### "nothing to commit, working tree clean"

Nothing has changed since your last commit. This is normal — no action needed.

### Accidentally Committed a Secret/Password

```bash
# Remove from last commit (if not pushed yet)
git reset --soft HEAD~1
# Remove the file or fix it, then re-commit

# If already pushed — change the password immediately, then use:
git filter-branch  # (advanced — look up "BFG Repo Cleaner")
```

### Pushed to Wrong Branch

```bash
# Undo the last push (creates a reverting commit — safe)
git revert HEAD
git push
```

---

## 13. Essential Git Commands Cheat Sheet

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SETUP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
git config --global user.name "Name"     Set your name
git config --global user.email "x@x.com" Set your email
git init                                  Create new local repo
git clone <url>                           Download a repo

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DAILY WORKFLOW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
git status                                See what changed
git add .                                 Stage all changes
git add <file>                            Stage one file
git commit -m "message"                   Save a snapshot
git push                                  Upload to GitHub
git pull                                  Download from GitHub

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BRANCHES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
git branch                                List branches
git checkout -b <branch>                  Create + switch branch
git checkout main                         Switch to main
git merge <branch>                        Merge branch into current
git branch -d <branch>                    Delete a branch

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HISTORY & INSPECTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
git log                                   Full history
git log --oneline                         Compact history
git diff                                  See unstaged changes
git show <commit>                         Inspect a commit

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
UNDOING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
git restore <file>                        Discard file changes
git restore --staged <file>               Unstage a file
git reset --soft HEAD~1                   Undo last commit (keep edits)
git revert HEAD                           Safely undo a pushed commit
```

---

## 14. Best Practices & Pro Tips

### The `.gitignore` File

This file tells Git to ignore certain files (secrets, build artifacts, OS junk). Place it in your project root.

**Example `.gitignore`:**

```
# Secrets
.env
*.key

# Python
__pycache__/
*.pyc
venv/

# Node.js
node_modules/
.npm

# macOS
.DS_Store

# Windows
Thumbs.db

# Build output
dist/
build/
```

> GitHub provides ready-made `.gitignore` templates when you create a repository. Always use one.

### Security Rules (Never Break These)

- ❌ Never commit `.env` files or files with passwords
- ❌ Never store API keys in your code
- ✅ Use environment variables for secrets
- ✅ Add `.env` to `.gitignore` immediately

### Commit Habits

- Commit often — small, focused commits are better than one giant commit
- One commit = one logical change
- Commit before switching tasks or taking a break
- Push at end of every work session

### Repository Hygiene

- Delete branches after merging them
- Keep your `main` branch always working/stable
- Tag important versions: `git tag v1.0.0`
- Archive repos you're no longer maintaining (GitHub Settings → Archive)

### Useful GitHub Features to Explore Next

|Feature|What it Does|
|---|---|
|**Issues**|Track bugs, to-dos, and feature requests|
|**Projects**|Kanban board for managing work|
|**Actions**|Automatically test/deploy code on every push|
|**Pages**|Host a free website directly from your repo|
|**Releases**|Package and publish versioned releases|
|**Wiki**|Add detailed documentation to your project|

---

## Quick Start Checklist

```
□ Install Git
□ Set git config name and email
□ Create GitHub account
□ Set up SSH key
□ Create a repository on GitHub
□ git init  (in your project folder)
□ git remote add origin <url>
□ Create a .gitignore file
□ git add .
□ git commit -m "Initial commit"
□ git push -u origin main
□ 🎉 Your project is live on GitHub!
```

---

_Guide version 1.0 — Created with Claude_ _For the latest Git documentation, visit [git-scm.com/docs](https://git-scm.com/docs)_ _For GitHub help, visit [docs.github.com](https://docs.github.com/)_