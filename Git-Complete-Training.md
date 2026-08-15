# Git Complete Training Course
### From Zero to Professional Version Control

---

# Table of Contents

1. [Session Overview](#session-overview)
2. [What is Version Control?](#what-is-version-control)
3. [What is Git?](#what-is-git)
4. [Git vs GitHub vs GitLab](#git-vs-github-vs-gitlab)
5. [Installing Git](#installing-git)
6. [Configure Git](#configure-git)
7. [Creating Your First Repository](#creating-your-first-repository)
8. [Git Workflow](#git-workflow)
9. [Basic Git Commands](#basic-git-commands)
10. [.gitignore](#gitignore)
11. [Professional Commit Messages](#professional-commit-messages)
12. [Branching](#branching)
13. [Merging](#merging)
14. [Merge Conflicts](#merge-conflicts)
15. [Remote Repositories](#remote-repositories)
16. [GitHub Operations](#github-operations)
17. [Pull Requests](#pull-requests)
18. [Forking](#forking)
19. [GitHub Pages](#github-pages)
20. [Undo Changes](#undo-changes)
21. [Stashing](#stashing)
22. [Tags](#tags)
23. [Git Aliases](#git-aliases)
24. [Git Best Practices](#git-best-practices)
25. [Git Project Workflow](#git-project-workflow)
26. [Team Collaboration](#team-collaboration)
27. [Practical Exercises](#practical-exercises)
28. [Git Interview Questions](#git-interview-questions)
29. [Common Mistakes](#common-mistakes)
30. [Command Cheat Sheet](#command-cheat-sheet)

---

# Session Overview

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

## Learning Objectives
By the end of this session, you will be able to:
- Understand version control concepts
- Initialize and configure Git repositories
- Use basic Git commands confidently
- Create and manage branches
- Handle merge conflicts
- Work with remote repositories
- Write professional commit messages
- Collaborate effectively using Git and GitHub

---

# What is Version Control?

## Definition
Version Control is a system that records changes to files over time so you can recall specific versions later.

## Why Version Control?
- Track history of changes
- Collaborate with others
- Revert to previous versions
- Compare different versions
- Backup your work

## Types of Version Control
- **Local**: One computer only
- **Centralized**: Single server (SVN)
- **Distributed**: Every computer has full history (Git)

## Without Git
```
Project
Project-New
Project-New2
Project-Final
Project-Final-Last
Project-Final-Real
```

## With Git
```
One Project
Unlimited History
```

---

# What is Git?

## Definition
Git is a **distributed version control system** created by **Linus Torvalds** in 2005.

## Key Features
- Distributed (every copy is a full repository)
- Fast and efficient
- Branching and merging
- Non-linear development
- Open source

## Who Uses Git?
- Front-End Developers
- Back-End Developers
- Full Stack Developers
- Mobile Developers
- DevOps Engineers
- QA Engineers
- Data Scientists

Git is one of the most requested skills in technical interviews.

---

# Git vs GitHub vs GitLab

## Git
A Version Control System that works locally on your computer.

## GitHub
A cloud platform for hosting Git repositories.
- Remote repositories
- Pull Requests
- Issues
- Actions (CI/CD)

## GitLab
Alternative to GitHub.
- Repository Hosting
- CI/CD
- Project Management

**Key Difference:** Git is the software/tool, GitHub/GitLab are hosting services/platforms.

---

# Installing Git

## Download Git from
https://git-scm.com

## Verify Installation
```bash
git --version
```

Example output:
```bash
git version 2.48.1
```

---

# Configure Git

## Configure Your Username
```bash
git config --global user.name "John Doe"
```

## Configure Your Email
```bash
git config --global user.email "john@example.com"
```

## Check Configuration
```bash
git config --list
```

---

# Creating Your First Repository

## Create a Folder
```
Git Course
```

## Open Terminal and Initialize Git
```bash
git init
```

Git creates a hidden folder `.git` that stores the project's history.

---

# Git Workflow

## The Four Stages
```
Working Directory
       ↓
   Staging Area
       ↓
  Local Repository
       ↓
  Remote Repository
```

## Workflow Commands
```
git add    (Working → Staging)
git commit (Staging → Local Repository)
git push   (Local → Remote)
```

## Three Main Areas
1. **Working Directory**: Where you make changes
2. **Staging Area**: Where you prepare commits
3. **Repository**: Where Git stores your commits

---

# Basic Git Commands

## git status
Check the status of your working directory.

```bash
git status
```

Shows:
- Untracked files
- Modified files
- Staged files
- Current branch

---

## git add
Stage files for commit.

```bash
# Add specific file
git add index.html

# Add all files
git add .

# Add all changed files
git add -A
```

---

## git commit
Save changes to repository.

```bash
git commit -m "Create homepage"
```

A commit saves a snapshot of your project.

---

## git log
View commit history.

```bash
git log

# One line per commit
git log --oneline

# Show graph
git log --graph --oneline

# Show changes
git log -p
```

---

## git diff
Show changes between commits.

```bash
# Show unstaged changes
git diff

# Show staged changes
git diff --staged

# Compare with specific commit
git diff commit-hash
```

---

## git mv
Rename file.

```bash
git mv old.txt new.txt
```

---

## git rm
Delete file.

```bash
git rm file.txt
```

---

# .gitignore

## What is .gitignore?
A file that tells Git which files/directories to ignore.

## Common .gitignore Entries
```gitignore
# Dependencies
node_modules/
package-lock.json

# Environment files
.env
.env.local

# IDE
.vscode/
.idea/

# OS files
.DS_Store
Thumbs.db

# Build files
dist/
build/
coverage/

# Logs
*.log
```

## Creating .gitignore
```bash
# Create file
touch .gitignore

# Or manually create .gitignore file
```

---

# Professional Commit Messages

## Conventional Commits Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

## Commit Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

## Examples
```
feat: add user authentication

Implement login and registration functionality
with JWT tokens and password hashing.

Closes #123
```

```
fix: resolve navigation bug on mobile

Fixed navigation menu not closing after
clicking on mobile devices.
```

## Good Commit Message Checklist
- [ ] Uses conventional commit format
- [ ] Type is appropriate (feat, fix, docs, etc.)
- [ ] Subject is clear and concise
- [ ] Subject uses present tense
- [ ] Subject doesn't end with period
- [ ] Body explains "why" not "what"
- [ ] References related issues (if applicable)

---

# Branching

## What is a Branch?
A branch is an independent line of development.

## Show Branches
```bash
git branch
```

## Create Branch
```bash
git branch login-page
```

## Switch Branch
```bash
git checkout login-page
# Or modern command
git switch login-page
```

## Create and Switch
```bash
git checkout -b login-page
# Or modern command
git switch -c login-page
```

## List Branches
```bash
git branch
```

## Delete Branch
```bash
git branch -d login-page
```

---

# Merging

## What is Merge?
Merge combines changes from different branches.

## Merge Branch
```bash
git checkout main
git merge login-page
```

Git combines the changes.

---

# Merge Conflicts

## What is a Merge Conflict?
A merge conflict happens when two branches edit the same code.

## Example
Branch A:
```html
<h1>Hello</h1>
```

Branch B:
```html
<h1>Welcome</h1>
```

Git asks you to choose the correct version.

## Resolve Conflicts
1. Open the conflicted file
2. Look for `<<<<<<<`, `=======`, `>>>>>>>`
3. Edit to resolve
4. `git add` the file
5. `git commit` to complete merge

---

# Remote Repositories

## What is a Remote?
A remote is a version of your project hosted on the internet or network.

## Add Remote
```bash
git remote add origin https://github.com/username/project.git
```

## View Remotes
```bash
git remote -v
```

## Remove Remote
```bash
git remote remove origin
```

---

# GitHub Operations

## git push
Upload local commits to remote.

```bash
git push origin main

# First time with upstream
git push -u origin main
```

## git pull
Download and merge remote changes.

```bash
git pull origin main
```

## git fetch
Download changes without merging.

```bash
git fetch
```

## Pull vs Fetch
- `pull`: Downloads and merges
- `fetch`: Downloads only (doesn't merge)

## git clone
Clone copies a remote repository to your local machine.

```bash
git clone https://github.com/username/project.git
```

## Clone to Specific Directory
```bash
git clone https://github.com/username/project.git my-folder
```

---

# Pull Requests

## What is a Pull Request?
A PR is a request to merge your branch into another branch.

## Typical Workflow
```
Create Branch
    ↓
Write Code
    ↓
Push Branch
    ↓
Open Pull Request
    ↓
Code Review
    ↓
Merge
```

## Creating PR
1. Push your branch to GitHub
2. Go to repository on GitHub
3. Click "New Pull Request"
4. Select branches to merge
5. Add description
6. Submit for review

## Reviewing PR
- Review code changes
- Add comments
- Request changes
- Approve
- Merge

## Benefits
- Code review
- Team collaboration
- Better quality

---

# Forking

## What is a Fork?
A fork is a copy of a repository under your own account.

## When to Fork
- Contributing to open source
- Experimenting with code
- Creating your own version

## Forking Process
1. Click "Fork" on GitHub
2. Clone your fork locally
3. Make changes
4. Push to your fork
5. Create PR to original repo

---

# GitHub Pages

## What is GitHub Pages?
Free static website hosting from GitHub repositories.

## Enable GitHub Pages
1. Go to repository Settings
2. Click "Pages"
3. Select branch (usually main or gh-pages)
4. Select folder (root or /docs)
5. Save

## Access Your Site
```
https://username.github.io/repository-name
```

---

# Undo Changes

## Restore File
```bash
git restore index.html
```

## Unstage File
```bash
git restore --staged index.html
```

## Undo Last Commit (Keep Changes)
```bash
git reset --soft HEAD~1
```

## Undo Last Commit (Remove Staged)
```bash
git reset --mixed HEAD~1
```

## Undo Completely
```bash
git reset --hard HEAD~1
```

---

# Stashing

## What is Git Stash?
Temporarily save unfinished work.

## Save
```bash
git stash
```

## Show Stashes
```bash
git stash list
```

## Restore
```bash
git stash pop
```

## Delete
```bash
git stash drop
```

---

# Tags

## What are Tags?
Tags mark releases.

## Create Tag
```bash
git tag v1.0.0
```

## List Tags
```bash
git tag
```

## Push Tags
```bash
git push origin --tags
```

---

# Git Aliases

## Create Aliases
```bash
git config --global alias.st status
```

Now instead of:
```bash
git status
```

You can write:
```bash
git st
```

## Another Example
```bash
git config --global alias.co checkout
```

---

# Git Best Practices

✔ Commit often

✔ Write meaningful commit messages

✔ Use branches

✔ Pull before pushing

✔ Never commit passwords

✔ Never commit API keys

✔ Use .gitignore

✔ Review code before pushing

✔ One feature per branch

✔ Keep commits small

---

# Git Project Workflow

## Example Workflow
```
Create Project
    ↓
git init
    ↓
Create Files
    ↓
git add .
    ↓
git commit
    ↓
Create GitHub Repository
    ↓
git remote add origin
    ↓
git push
    ↓
Create Feature Branch
    ↓
Develop Feature
    ↓
Commit Changes
    ↓
Push Branch
    ↓
Open Pull Request
    ↓
Merge
    ↓
Delete Branch
```

---

# Team Collaboration

## Best Practices

### 1. Pull Before Push
```bash
git pull origin main
git push origin main
```

### 2. Use Branches for Features
- Don't work directly on main
- Create feature branches
- Merge via pull requests

### 3. Write Clear PR Descriptions
- Explain what changes
- Why the changes are needed
- How to test

### 4. Review Code Before Merging
- Check for bugs
- Ensure code quality
- Verify tests pass

### 5. Resolve Conflicts Promptly
- Don't leave conflicts unresolved
- Communicate with team
- Test after merging

---

# Practical Exercises

## Exercise 1: Initialize Git Repository (20 minutes)

### Task
Initialize a Git repository for a project and make commits.

```bash
# Create project directory
mkdir my-project
cd my-project

# Initialize Git
git init

# Create .gitignore
echo "node_modules/" > .gitignore
echo ".env" >> .gitignore

# Create files
echo "# My Project" > README.md
echo "console.log('Hello World');" > index.js

# Check status
git status

# Stage files
git add .

# Commit
git commit -m "feat: initial project setup"

# Create feature branch
git checkout -b add-feature

# Add new file
echo "New feature" > feature.js

# Stage and commit
git add .
git commit -m "feat: add new feature"

# Switch back to main
git checkout main

# Merge feature
git merge add-feature

# View log
git log --oneline
```

---

## Exercise 2: Remote Repository & Push (25 minutes)

### Task
Create a repository on GitHub and push your code.

```bash
# Go to GitHub and create new repository
# Copy the repository URL

# Add remote
git remote add origin https://github.com/username/my-project.git

# Push to GitHub
git push -u origin main

# Push all branches
git push --all origin
```

### GitHub Steps
1. Go to github.com
2. Click "+" → "New repository"
3. Enter repository name
4. Make it public or private
5. Click "Create repository"
6. Copy the URL
7. Add remote and push

---

## Exercise 3: Branching and Merging (25 minutes)

### Task
Create multiple branches and merge them.

```bash
# Create main branch content
echo "Main content" > main.txt
git add .
git commit -m "feat: add main content"

# Create feature-1 branch
git checkout -b feature-1
echo "Feature 1 content" > feature1.txt
git add .
git commit -m "feat: add feature 1"

# Create feature-2 branch from main
git checkout main
git checkout -b feature-2
echo "Feature 2 content" > feature2.txt
git add .
git commit -m "feat: add feature 2"

# Merge feature-1 to main
git checkout main
git merge feature-1

# Merge feature-2 to main
git merge feature-2

# Delete feature branches
git branch -d feature-1
git branch -d feature-2

# View history
git log --graph --oneline
```

---

## Exercise 4: Professional Commit Messages (20 minutes)

### Task
Practice writing professional commit messages.

```bash
# Feature commit
git add index.js
git commit -m "feat: add user authentication module"

# Bug fix commit
git add utils.js
git commit -m "fix: resolve memory leak in data processing"

# Documentation commit
git add README.md
git commit -m "docs: update installation instructions"

# Style commit
git add styles.css
git commit -m "style: format CSS according to style guide"

# Refactor commit
git add app.js
git commit -m "refactor: simplify user validation logic"

# Test commit
git add test.js
git commit -m "test: add unit tests for auth module"

# Chore commit
git add package.json
git commit -m "chore: update dependencies to latest versions"
```

---

# Git Interview Questions

### What is Git?
A distributed version control system.

### Difference between Git and GitHub?
Git is the software. GitHub hosts Git repositories online.

### What is a repository?
A project managed by Git.

### What is a commit?
A snapshot of your project.

### What is HEAD?
The current commit you are working on.

### Difference between fetch and pull?
Fetch downloads changes. Pull downloads and merges changes.

### Difference between merge and rebase?
Merge creates a merge commit. Rebase rewrites commit history for a cleaner timeline.

### What is Git Stash?
Temporary storage for uncommitted changes.

### What is .gitignore?
A file listing files Git should ignore.

### Difference between reset, revert, and restore?
- **restore**: Restore files
- **reset**: Move HEAD and optionally remove commits
- **revert**: Create a new commit that undoes previous changes

### What does `git add` do?
Stages files for commit, moving them from working directory to staging area.

### What is a Git branch?
An independent line of development that allows parallel work on different features.

### What is a Pull Request?
A request to merge your branch into another branch, usually after code review.

### What is a Fork?
A copy of a repository under your own account, used for contributing to open source.

### How do you enable GitHub Pages?
Go to repository Settings → Pages → select branch and folder → Save.

---

# Common Mistakes

❌ Forgetting to commit regularly

❌ Using "Final_Final_Last.zip"

❌ Committing `.env`

❌ Committing `node_modules`

❌ Working directly on `main`

❌ Using meaningless commit messages

```
Update
Fix
Test
123
```

## Better Messages
```
Add login page
Fix navbar alignment
Implement authentication
Update user profile validation
```

---

# Command Cheat Sheet

```bash
# Repository Setup
git init
git clone

# Status & Information
git status
git log
git log --oneline
git log --graph --oneline

# Staging & Committing
git add .
git add filename
git commit -m "message"

# Changes & Diff
git diff
git diff --staged
git diff commit-hash

# Branching
git branch
git branch branch-name
git checkout branch-name
git switch branch-name
git checkout -b branch-name
git switch -c branch-name
git branch -d branch-name

# Merging
git merge branch-name

# Remote Operations
git remote -v
git remote add origin URL
git remote remove origin
git fetch
git pull
git push
git push -u origin main
git push --all origin

# Stashing
git stash
git stash list
git stash pop
git stash drop

# Tags
git tag
git tag v1.0.0
git push origin --tags

# Undo Operations
git restore filename
git restore --staged filename
git reset --soft HEAD~1
git reset --mixed HEAD~1
git reset --hard HEAD~1
git revert

# File Operations
git mv old.txt new.txt
git rm file.txt
```

---

# Best Learning Strategy

1. Learn one command at a time
2. Practice immediately
3. Create a small project
4. Make mistakes and fix them
5. Use Git every day
6. Push projects to GitHub
7. Collaborate with others
8. Read commit history
9. Review pull requests
10. Never stop practicing

---

# Final Project

## Build a Simple Website with Git

### Tasks:
- Initialize a Git repository
- Create `index.html`
- Create `style.css`
- Commit the initial project
- Create a `navbar` branch
- Add a navigation bar
- Commit the changes
- Merge the branch into `main`
- Push the project to GitHub
- Create a Pull Request
- Tag the first release as `v1.0.0`

---

# Homework Assignment

## Task
Set up a Git repository for a project and push to GitHub.

## Requirements:
- Initialize Git repository
- Create proper .gitignore
- Make at least 5 commits with professional messages
- Create and merge at least 2 branches
- Push to GitHub
- Create a comprehensive README
- Enable GitHub Pages (if applicable)

## Due Date
Next session

---

# README Template

```markdown
# Project Name

## Description
Brief description of the project

## Installation
How to install dependencies

## Usage
How to use the project

## Features
List of features

## Contributing
How to contribute

## License
Project license
```

---

# Practice Challenges

## Challenge 1
Create a repository with proper .gitignore and make initial commit.

## Challenge 2
Create a feature branch, make changes, and merge it back to main.

## Challenge 3
Write professional commit messages for a series of changes.

---

Congratulations! 🎉

You now have the foundational Git skills used in professional software development.

---

**Next Session:** Terminal & npm (Basic terminal commands, npm, package.json, dependencies, npx)